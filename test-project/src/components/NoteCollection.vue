<template>
notes
    <div v-for="note in notes" :key="note">
note
        <SingleNote :note="note"></SingleNote>
    </div>

</template>

<script>
    import singleNote from "./singleNote"
export default {
  data: () => {
        return {
        notes: [
        { note: 'B1' },
        { note: 'B2' },
        { note: 'B3' },
        { note: 'B4' },
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
</style>
