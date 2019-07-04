<template>
    <div id="application">
        <button v-on:click="generateMelody">generate melody</button><br>
        <button v-on:click="start">start</button><br>
        <button v-on:click="stop">stop</button><br>
        <button v-on:click="clear">clear</button><br>
        <table>
                <NoteCollection v-for="(melody,index) in melodies" :key="`melody-${index}`" :notes="melody.notes" :clearNote="clearNote" :index="index" />
        </table>

    </div>
</template>

<script src="js/tonejs-ui.js"></script>
<script src="js/tone.js"></script>

<script>
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
                this.synth.connect(new Tone.Reverb(1))
                var total = this.noteChances.reduce(this.sum);
                var melody = { notes: new Array() };
                for (let noteIndex = 0; noteIndex < this.restChances.length; noteIndex++) {
                    var rand = Math.random() * total;
                    var index = 0;
                    var totalChance = 0;
                    while (index < this.noteChances.length) {
                        totalChance += this.noteChances[index];
                        if (totalChance >= rand) {
                            melody.notes.push(({ note: this.cMinor[index], isActive: false, synth: this.synth }));
                            break;
                        }
                        index++;
                    }
                }
                this.addRests(melody.notes);
                this.removeDuplicates(melody.notes);
                this.setPreviousNotes(melody.notes);
                const synthPart = new Tone.Sequence(
                    function (time, note) {
                        if (note.note) {
                            note.synth.triggerAttackRelease(note.note, "10hz", time);
                        }
                        note.isActive = true;
                        note.previousNote.isActive = false;
                    },
                    melody.notes,
                    "4n"
                )
                var vueMelody = melody
                this.melodies.push(vueMelody);
                this.sequences.push(synthPart);
                synthPart.start();
                //Tone.Transport.position="1:1:1";
            },
            setPreviousNotes: function (notes) {
                
                notes[0].previousNote = notes[notes.length - 1];
                
                for (let i = 1; i < notes.length; i++) {
                    notes[i].previousNote = notes[i - 1];
                }
            },
            removeDuplicates: function (melody) {
                for (let i = 0; i < melody.length; i++) {
                    for (let m = 0; m < this.melodies.length; m++) {
                        if (this.melodies[m].notes.length > i) {
                            var noteAtTime = this.melodies[m].notes[i];
                            if (noteAtTime) {
                                if (melody[i].note == noteAtTime.note) {
                                    melody[i].note = null;
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
                        melody[index].note = null;
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
                for (let m = 0; m < this.melodies.length; m++) {
                    for (let n = 0; n < this.melodies[m].length; n++) {
                        this.melodies[m].notes[n].isActive = false;
                    }
                }
                //Tone.Transport.position="1:1:1";
            },
            clearNote: function (note,rowIndex,noteIndex) {
                alert(note.note + " isActive? " + note.isActive+"\n"+rowIndex+","+noteIndex);
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
