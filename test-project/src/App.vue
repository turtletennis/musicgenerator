<template>
    <div id="app">
        <table>
            <tr v-for="melody in melodies" :key="melody">
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
                melodies: [
                    {
                        notes: [
                            { note: 'B1', isActive: true },
                            { note: 'B2' },
                            { note: 'B3' },
                            { note: 'B4' },
                        ]
                    },
                    {
                        notes: [
                            { note: 'C1' },
                            { note: 'D2' },
                            { note: 'E3' },
                            { note: 'F4' },
                        ]
                    }
                ],
                synth: new Tone.PolySynth(6, Tone.Synth).toMaster(),
                cMinor: ["C3", "D3", "Eb3", "F3", "G3", "Ab3", "B3", "C4"],
                noteChances: [1.0, 0.5, 0.7, 0.6, 0.9, 0.6, 0.3, 0.9],
                restChances: [0.1, 0.4, 0.3, 0.2, 0.3, 0.4, 0.3, 0.4, 0.2, 0.4, 0.3, 0.2, 0.3, 0.4, 0.3, 0.4],
                allSequences: []
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
                for (let note = 0; note < this.restChances.length; note++) {
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
                this.melodies.push(
                    melody.map(m => ({
                        note: m,
                        isActive: false
                    })
                    ));
                this.allSequences.push(synthPart);
                synthPart.start();

            },
            removeDuplicates: function (melody) {
                for (let i = 0; i < melody.length; i++) {
                    for (let m = 0; m < this.melodies.length; m++) {
                        if (this.melodies.length > i) {
                            if (melody[i] == this.melodies[m][i].note) {
                                melody[i] = null;
                                break;
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
                this.allMelodies = [];
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
