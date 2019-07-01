<template>
  <div id="app">

    <NoteCollection></NoteCollection>

  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import SingleNote from './components/SingleNote'
import tone from 'tone'
import doStuff, { doSomethingElse } from './utils';

doSomethingElse(10);


export default {
  name: 'app',
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
  components: {
      SingleNote,
      NoteCollection
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
