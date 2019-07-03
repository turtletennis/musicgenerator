<template>
    <td class="note" :class="{ 'highlight': note.isActive }" v-on:click="clearNote(note,rowIndex,noteIndex)">{{ note.note }}</td>
</template>

<script>
export default {
  data: () => {
    return {
    }
  },
  props: {
      note: Object,
      clearNote: Function,
      noteIndex: Number,
      rowIndex: Number
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
</style>
