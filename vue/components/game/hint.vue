<template lang="pug">
    #philosophers-intro
        template(v-if="game")
            h1.uk-text-center {{ "Die letzte Antwort war leider falsch!" }}
            h3.uk-text-center {{ "Sie haben die Möglichkeit am Ende der Runde falsch beantwortete Fragen noch einmal zu beantworten. Für den zweiten Versuch erhalten Sie jedoch keine Punkte." }}
            h3.uk-text-center {{ "Wählen Sie dazu die Fragen am Ende der Runde erneut an." }}
            
        p.uk-text-center
            button.btn.btn-primary(@click="startGame")
                span {{ "Weiter!" }}
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
                'setGameMode',
                'setHintSeen'
            ]),
            startGame() {
                this.setHintSeen(true);
                this.setGameMode(MODE_LEVELS);
            },
          
            
        }
    }
</script>
