<template>
  <random-xkcd></random-xkcd>
</template>

<script>
import RandomXkcd from './components/RandomXkcd.vue';

export default {
  name: 'App',
  components: {
    RandomXkcd,
  }
}

</script>

<style lang="scss">
//Font Poppins & Comic Neue
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Comic+Neue:wght@300&display=swap');

#app {

}
</style>
