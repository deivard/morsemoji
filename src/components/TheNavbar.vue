<template>
  <div id="wrapper">
    <h1 id="title"
      class="text-4xl mb-0"
    >
      morsemoji
    </h1>
    <h2 id="quote"
      class="md:text-sm text-center italic font-thin"
    >
      Why not?  
    </h2>
  </div>

</template>

<script>
export default {
  name: 'TheNavbar',
  props: {

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#wrapper {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
}
#title {
  text-align: center;
  grid-column: 1/13;
  grid-row: 1;
}
#quote {
  grid-row: 2;
  grid-column: 1/13;
  
}
h5 {
  text-align: center;

}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
