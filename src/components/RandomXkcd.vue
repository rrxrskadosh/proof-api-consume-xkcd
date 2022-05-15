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
            <h2>{{current.safe_title}}</h2>
            <img v-bind:src="current.img" v-bind:alt="current.alt">
            <p>{{current.alt}}</p>
            <p><i>#{{current.num}}, drawn on {{current.day}} {{month}} {{current.year}}</i></p>
            <star-rating></star-rating>
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
  },
  data() {
     return {
      number: '',
      max: '',
      current: {
      title: '',
      img: '',
      alt: ''
    },
    loading: true,
     }
  },

  computed: {
    month() {
      var month = new Array;
      if (this.current.month === undefined)
        return '';
      month[0] = "January";
      month[1] = "February";
      month[2] = "March";
      month[3] = "April";
      month[4] = "May";
      month[5] = "June";
      month[6] = "July";
      month[7] = "August";
      month[8] = "September";
      month[9] = "October";
      month[10] = "November";
      month[11] = "December";
      return month[this.current.month - 1];
    },
  },
  watch: {
    number(value, oldvalue) {
      if (oldvalue === '') {
        this.max = value;
      } else {
        this.xkcd();
      }
    },
  },
  created() {
    this.xkcd();
  },
  methods: {
    async xkcd() {
      try {
        this.loading = true;
        let url = 'https://xkcd.now.sh/?comic=';
        if (this.number === '') {
          url += 'latest';
        } else {
          url += this.number;
        }
        const response = await axios.get(url);
        this.current = response.data;
        this.loading = false;
        this.number = response.data.num;
      } catch (error) {
        this.number = this.max;
      }
    },
    getRandom(min, max) {
      min = Math.ceil(min);
      max = Math.floor(max);
      return Math.floor(Math.random() * (max - min + 1)) + min; //The maximum and minimum are inclusive
    },
    randomComic() {
      this.number = this.getRandom(1, this.max);
    },
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