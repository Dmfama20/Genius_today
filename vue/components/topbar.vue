<template lang="pug">
    .uk-grid.uk-grid-collapse.top-bar(uk-grid).uk-flex-middle
        .uk-width-expand
            button.btn.uk-margin-small-left(@click="restartGame", :class="{'btn-primary': isGameOver, 'btn-default': !isGameOver}")
                v-icon(name="redo").uk-margin-small-right
                span(v-if="isGameOver" ) {{ "Nächste Stufe!" }}
                span(v-if="!isGameOver")  {{ "Spiel läuft..." }}

            button.btn.btn-default.uk-margin-small-left(@click="showHighscore", v-if="highscoreButtonVisible")
                v-icon(name="chart-line").uk-margin-small-right
                span {{ strings.game_btn_highscore }}&nbsp;
            span(v-if="isModeLevels && this.game.sequential_question_ordering && this.isAdminUser") {{" Stufe wählen: "}}
            select.uk-select.uk-form-width-xsmall(v-model="selectedChapter",@change="chapterchange",v-if="isModeLevels &&  this.game.sequential_question_ordering && this.isAdminUser")
                option( v-for="index in this.game.questions_quantity" :key="index", value=index) {{ index }}







            button.btn.btn-default.uk-margin-small-left(@click="showGame", v-if="gameButtonVisible")
                v-icon(name="gamepad").uk-margin-small-right
                span {{ strings.game_btn_game }}

        .uk-width-auto
            button.btn.btn-default.uk-margin-small-right(@click="showAdmin", v-if="adminButtonVisible")
                v-icon(name="cogs")
            button.btn.btn-default.uk-margin-small-right(@click="showHelp")
                v-icon(name="regular/question-circle")
</template>

<script>
    import {mapActions, mapMutations, mapState} from 'vuex';
    import {GAME_PROGRESS, MODE_HELP,  MODE_LEVELS, MODE_QUESTION, MODE_HIGHSCORE} from "../constants";
    import _ from 'lodash';
    import mixins from '../mixins';

    export default {
     data() {
            return {
                selectedChapter: 1,

                           }
        },
        mixins: [mixins],
        computed: {
            ...mapState([
                'strings',
                'game',
                'gameMode',
                'gameSession',
                'question',
                'chapter',
                'secondChance',
                'isWrongAnswer'
            ]),
            isAdminUser() {
                return this.game.mdl_user_teacher;
            },
            isAdminScreen() {
                return this.$route.name === 'admin-screen';
            },
            isGameScreen() {
                return this.$route.name === 'game-screen';
            },
            isGameOver() {
                return this.gameSession === null || this.gameSession.state !== GAME_PROGRESS;
            },
            isModeLevels() {
            return this.gameMode == MODE_LEVELS;
            },
            highscoreButtonVisible() {
                return this.gameMode !== MODE_HIGHSCORE || !this.isGameScreen;
            },
            gameButtonVisible() {
                if (this.gameSession === null || this.question === null) {
                    return false;
                }
                let modes = [MODE_HIGHSCORE, MODE_HELP];
                return _.includes(modes, this.gameMode) || !this.isGameScreen;
            },
            adminButtonVisible() {
                return this.isAdminUser && !this.isAdminScreen;
            },
            isWrongAnswerGiven() {
                return this.isWrongAnswer;
            }
        },
        methods: {
            ...mapActions([
                'createGameSession'
            ]),
            ...mapMutations([
                'setGameMode',
                'choseChapter',
                'setSecondChance',
                'setisWrongAnswer',
                'resetCountAnswers',
                'setfinalscore',
                'setHintSeen'
            ]),
            	onchange() {

                    console.log(this.selectedChapter);

                        },
                chapterchange() {

                    this.choseChapter(this.selectedChapter);

                        },


            restartGame() {

                if(!this.isGameOver) {

                    //The current game cannot be restarted!
                    this.setHintSeen(false);
                    this.setisWrongAnswer(false);
                    this.resetCountAnswers();
                    this.setSecondChance(false);
                    this.createGameSession();
                    this.goToGameRoute();

                }
                else {
                    this.setHintSeen(false);
                    this.setfinalscore(0);
                    this.setisWrongAnswer(false);
                    this.resetCountAnswers();
                   this.setSecondChance(false);
                    this.selectedChapter++;
                    this.choseChapter(this.selectedChapter);
                    this.createGameSession();
                    this.goToGameRoute();
                }




            },
            showHighscore() {
                this.setGameMode(MODE_HIGHSCORE);
                this.goToGameRoute();
            },
            showGame() {
                if (!this.question || this.question.finished) {
                    this.setGameMode(MODE_LEVELS);
                } else {
                    this.setGameMode(MODE_QUESTION)
                }
                this.goToGameRoute();
            },
            showAdmin() {
                this.goToAdminRoute();
            },
            showHelp() {
                this.setGameMode(MODE_HELP);
                this.goToGameRoute();
            },
            goToAdminRoute() {
                if (!this.isAdminScreen) {
                    this.$router.push({name: 'admin-screen'});
                }
            },
            goToGameRoute() {
                if (!this.isGameScreen) {
                    this.$router.push({name: 'game-screen'});
                }
            }


      },


    }
</script>

<style scoped>
    .top-bar {
        background-color: #f8f8f8;
        border-top-left-radius: 5px;
        border-top-right-radius: 5px;
        border: 1px solid #ccc;
        height: 50px;
    }
</style>
