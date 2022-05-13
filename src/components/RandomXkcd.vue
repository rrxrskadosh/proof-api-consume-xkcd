<template>
  <html>
    <head><title>XKCD Random</title></head>
    <body>
      <h1>XKCD Random</h1>
      <div id="app">
        <div v-if="loading">
          <p>Loading...</p>
        </div>
        <div class="container" v-else>
          <div class="row">
            <p><button class="btn-random" v-on:click="randomComic">Random</button></p>
            <h2>Here Title Comic</h2>
            <img />
            <p>Alt</p>
            <p><i>Icons</i></p>
            <star-rating></star-rating>
            <p>Averate Rating:</p>
          </div>
        </div>
      </div>
    </body>
  </html>
</template>

<script>
import StarRating from './StarRating.vue';

export default {
  name: 'RandomXkcd',
  components: {
    StarRating,
  }
}
</script>

<style lang="scss">
* {
    box-sizing: border-box;
  }
  
  html,
  body {
    height: 100%;
  }

  .container {

  }

  .btn-random {
    background: red;
  }

</style>