<template>
    <div id="app">
        <button v-on:click="generateMelody">generate melody</button>
        <table>
            <tr v-for="(melody,index) in melodies" :key="`melody-${index}`">
                <NoteCollection :notes="melody.notes" ></NoteCollection>
            </tr>
        </table>

    </div>
</template>

<script src="js/tonejs-ui.js"></script>
<script>
    import NoteCollection from './components/NoteCollection'
    import Tone from 'tone'
    export default {
        name: 'app',
        data: () => {
            return {
                melodies: [],
                synth: new Tone.PolySynth(6, Tone.Synth).toMaster(),
                cMinor: ["C3", "D3", "Eb3", "F3", "G3", "Ab3", "B3", "C4"],
                noteChances: [1.0, 0.5, 0.7, 0.6, 0.9, 0.6, 0.3, 0.9],
                restChances: [0.1, 0.4, 0.3, 0.2, 0.3, 0.4, 0.3, 0.4, 0.2, 0.4, 0.3, 0.2, 0.3, 0.4, 0.3, 0.4],
                Sequences: []
            }
        },
        components: {
            NoteCollection
        },
        methods: {
            generateMelody: function () {
                this.synth.connect(new Tone.Reverb(1))
                var total = this.noteChances.reduce(this.sum);
                var melody = new Array();
                for (let noteIndex = 0; noteIndex < this.restChances.length; noteIndex++) {
                    var rand = Math.random() * total;
                    var index = 0;
                    var totalChance = 0;
                    while (index < this.noteChances.length) {
                        totalChance += this.noteChances[index];
                        if (totalChance >= rand) {
                            melody.push(this.cMinor[index]);
                            break;
                        }
                        index++;
                    }
                }
                this.addRests(melody);
                this.removeDuplicates(melody);
                const synthPart = new Tone.Sequence(
                    function (time, note) {
                        this.synth.triggerAttackRelease(note, "10hz", time);
                    },
                    melody,
                    "4n"
                )
                var vueMelody = {notes:[]};
                vueMelody.notes = melody.map(m => ({
                        note: m,
                        isActive: false
                    }));
                this.melodies.push(vueMelody);
                this.Sequences.push(synthPart);
                synthPart.start();

            },
            removeDuplicates: function (melody) {
                for (let i = 0; i < melody.length; i++) {
                    for (let m = 0; m < this.melodies.length; m++) {
                        if (this.melodies.length > i) {
                            var noteAtTime = this.melodies[m].notes[i];
                            if (noteAtTime) {
                                if (melody[i] == noteAtTime.note) {
                                    melody[i] = null;
                                    break;
                                }
                            }
                        }
                    }
                }
                //TODO: check other melodies and remove duplicate notes
            },
            addRests: function (melody) {

                var index = 0;
                while (index < this.noteChances.length) {
                    var rand = Math.random();
                    if (this.restChances[index] >= rand) {
                        melody[index] = null;
                    }
                    index++;
                }
            },
            sum: function (total, value, index, array) {
                return total + value;
            },
            clear: function () {
                for (let i = 0; i < this.allSequences.length; i++) {
                    var sequence = this.allSequences.pop();
                    sequence.stop();
                    sequence.removeAll();
                    sequence.dispose();
                }
                this.melodies = [];
                this.classes = [];
                Tone.Transport.clear();
                Tone.Transport.cancel();
            }
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
