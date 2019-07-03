<template>
    <tr>
        <SingleNote v-for="(note,index) in notes" :key="`note-${index}`" :note="note" :class="{ 'highlight': note.isActive }"/>
    </tr>
</template>

<script>
    import SingleNote from './SingleNote'
    export default {
        data: () => {
            return {
            }
        },
        props: {
            notes: Array
        },
        components: {
            SingleNote
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
