<template>
    <div id="application">
        <button v-on:click="generateMelody">generate melody</button><br>
        <button v-on:click="start">start</button><br>
        <button v-on:click="stop">stop</button><br>
        <button v-on:click="clear">clear</button><br>
        <table>
                <NoteCollection v-for="(melody,index) in melodies" :key="`melody-${index}`" :notes="melody.notes" />
        </table>

    </div>
</template>

<script src="js/tonejs-ui.js"></script>
<script src="js/tone.js"></script>

<script>
var synth = new Tone.PolySynth(6, Tone.Synth).toMaster();
window.synth = synth;
    import NoteCollection from './components/NoteCollection'
    import Tone from 'tone'
    export default {
        name: 'application',
        data: () => {
            return {
                melodies: [],
                synth: new Tone.PolySynth(6, Tone.Synth).toMaster(),
                cMinor: ["C3", "D3", "Eb3", "F3", "G3", "Ab3", "B3", "C4"],
                noteChances: [1.0, 0.5, 0.7, 0.6, 0.9, 0.6, 0.3, 0.9],
                restChances: [0.1, 0.4, 0.3, 0.2, 0.3, 0.4, 0.3, 0.4, 0.2, 0.4, 0.3, 0.2, 0.3, 0.4, 0.3, 0.4],
                sequences: []
            }
        },
        components: {
            NoteCollection
        },
        methods: {
            generateMelody: function () {
                window.synth.connect(new Tone.Reverb(1))
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
                        window.synth.triggerAttackRelease(note, "10hz", time);
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
                this.sequences.push(synthPart);
                synthPart.start();
                //Tone.Transport.position="1:1:1";
            },
            removeDuplicates: function (melody) {
                for (let i = 0; i < melody.length; i++) {
                    for (let m = 0; m < this.melodies.length; m++) {
                        if (this.melodies[m].notes.length > i) {
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
                for (let i = 0; i < this.sequences.length; i++) {
                    var sequence = this.sequences.pop();
                    sequence.stop();
                    sequence.removeAll();
                    sequence.dispose();
                }
                this.melodies = [];
                this.classes = [];
                Tone.Transport.clear();
                Tone.Transport.cancel();
            },
            start: function () {
                Tone.Transport.start();
            },
            stop: function () {
                Tone.Transport.stop();
                //Tone.Transport.position="1:1:1";
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
