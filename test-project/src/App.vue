<template>
  <div id="app">

    <NoteCollection></NoteCollection>

  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
    import SingleNote from './components/SingleNote'
    import NoteCollection from './components/NoteCollection'
import tone from 'tone'
import doStuff, { doSomethingElse } from './utils';

doSomethingElse(10);


export default {
  name: 'app',
  data: () => {
    return {
      
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
