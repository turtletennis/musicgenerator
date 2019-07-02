<template>
    <div>
        <td class="note" v-for="(note,index) in notes" :key="`note-${index}`"  :class="{ 'highlight': note.isActive }">
            {{note.note}}
        </td>
    </div>
</template>

<script>
    
    export default {
        data: () => {
            return {
            }
        },
        props: {
            notes: Array
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

    .highlight {
        color: #ff0000
    }
</style>
