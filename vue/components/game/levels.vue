<template lang="pug">
    vk-grid(matched)
        div(v-for="level in levels", :key="level.id", class="uk-width-1-1@s uk-width-1-2@m uk-width-1-3@l uk-width-1-4@xl")
            level(:level="level", :strings="strings", :game="game", @onSelectLevel="selectLevel")
</template>

<script>
    import {mapActions, mapState, mapMutations} from 'vuex';
    import mixins from '../../mixins';
    import Level from "../helper/level";

    export default {
        mixins: [mixins],
        computed: {
            ...mapState([
                'strings',
                'levels',
                'game',
                 'chapter',
                 'countAnswers',
                 'gameSession',
                 'finalscore',
                 'secondChance'
            ]),
             totalLevels() {
                return this.levels.length;
            },
             countedAnswers() {
                return this.countAnswers;
            },
              isSecondChance() {
                return this.secondChance;
            },

        },
        methods: {
            ...mapActions([
                'showQuestionForLevel',
            ]),
             ...mapMutations([
                'incrementCountAnswers',
                'setSecondChance',
                'setfinalscore'
            ]),
            selectLevel(levelId) {
                this.incrementCountAnswers();

                if (!this.isSecondChance){
                    if (this.countedAnswers>this.totalLevels)
                {
                    this.setSecondChance(true);
                    this.setfinalscore(this.gameSession.score);
                }

                }
                
               
                this.showQuestionForLevel(levelId);
            //     console.log("Chapter = " );
            //   console.log(this.chapter);
            },
        },
        components: {Level}
    }
</script>
