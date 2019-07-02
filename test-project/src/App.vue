<template>
  <div id="app">
      <table>
          <tr v-for="melody in melodies">
              <NoteCollection :notes="melody.notes"></NoteCollection>
          </tr>
      </table>

  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
    import NoteCollection from './components/NoteCollection'
import tone from 'tone'

export default {
  name: 'app',
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
  components: {
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
