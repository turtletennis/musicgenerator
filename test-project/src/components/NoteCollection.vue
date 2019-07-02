<template>
    <div>
        <td class="note" v-for="note in notes" :key="note"  :class="{ 'highlight': note.isActive }">
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
            notes: Object
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
