<template lang="pug">
    #philosophers-intro
        template(v-if="game")
            h1.uk-text-center(v-if="!secondChance") {{ game.name }}
            h3.uk-text-center(v-if="secondChance") {{ "Einige Frage wurde falsch beantwortet. Sie müssen alle Fragen korrekt beantworten um die nächste Stufe zu erreichen!" }}

            
        p.uk-text-center
            button.btn.btn-primary(@click="startGame")
                span(v-if="!secondChance")  {{ strings.game_btn_start }}
                span(v-if="secondChance")  {{ "Bonusrunde starten!" }}
</template>

<script>
    import {mapMutations, mapState} from 'vuex';
    import mixins from '../../mixins';
    import {MODE_LEVELS} from "../../constants";

    export default {
        mixins: [mixins],
        computed: {
            ...mapState([
                'strings',
                'game',
                'secondChance'
            ]),
        },
        methods: {
            ...mapMutations([
                'setGameMode'
            ]),
            startGame() {
                this.setGameMode(MODE_LEVELS);
            },
          
            
        }
    }
</script>
