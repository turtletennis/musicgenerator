<template>
    <div id="notes">
        <table>
            <tr v-for="melody in melodies">
                <td class="note" v-for="note in melody.notes" :class="{ 'highlight': note.isActive }">
                    {{note.note}}
                </td>
            </tr>
        </table>
    </div>

</template>

<script>
    import singleNote from "./SingleNote"
export default {
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
            ]
    }
  },
  props: {
    note: Object
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .note {
            width: 30px;
            max-width: 30px;
            height: 30px;
            max-height: 30px;
            border-style: solid;
        }
    .highlight{
        color:#ff0000
    }
</style>
