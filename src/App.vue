<template>
  <div id="app">
    <the-navbar class=""></the-navbar>
    <div id="bodyWrapper"
      class="flex flexwrap justify-center"
    >
      <div id="morseWrapper"
        class="text-center mt-10"
      >
        <Morse> </Morse>
      </div>
    </div>
  </div>
</template>

<script>
import TheNavbar from './components/TheNavbar.vue'
import Morse from './components/Morse.vue'

export default {
  name: 'app',
  components: {
    TheNavbar,
    Morse
  },
  data() {
    return {

    }
  },
  computed: {

  },
  methods: {

  },
  created() {

  }
}
</script>

<style>
* {
  font-family: 'Montserrat', sans-serif;
  color: #2c3e50;
}
body {
  background: #eeeeee;
}
#app {
  /* font-family: 'Avenir', Helvetica, Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
#bodyWrapper {
  /* display: grid;
  grid-template-columns: repeat(12, 1fr); */
}

#morseWrapper {
  grid-column: 4/9;
}

#categoriesWrapper {
  max-width: 45rem;
}

</style>
