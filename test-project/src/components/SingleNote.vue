<template>

  <td class="note">{{ note.note }}</td>

</template>

<script>
export default {
  data: () => {
    return {
    }
  },
  props: {
    note: Object
  }
}
</script>
.note {
            width: 30px;
            max-width: 30px;
            height: 30px;
            max-height: 30px;
            border-style: solid;
        }
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
