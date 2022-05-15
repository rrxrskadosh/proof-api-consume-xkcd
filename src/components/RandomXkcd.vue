<template>
  <html>
    <head><title>XKCD Random</title></head>
    <body>
      <h1 class="main-title">XKCD Random</h1>
      <div id="app">
        <div v-if="loading">
          <p class="loading">Loading...</p>
        </div>
        <div class="container" v-else>
          <div class="row">
            <div class="main-column">
              <p><button class="btn-random" v-on:click="randomComic">Random</button></p>
              <h2 class="secondary-title">{{current.safe_title}}</h2>
              <img class="img-comic" v-bind:src="current.img" v-bind:alt="current.alt">
              <div class="content-text-comic">
                <p class="text-alt">{{current.alt}}</p>
                <p class="text-comic"><i>#{{current.num}}, drawn on {{current.day}} {{month}} {{current.year}}</i></p>
              </div>
              <star-rating></star-rating>
            </div>
          </div>
        </div>
      </div>
    </body>
  </html>
</template>

<script>
import axios from "axios"; //Import Axios
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
    background: rgb(33,175,230);
    background: linear-gradient(90deg, rgba(33,175,230,1) 12%, rgba(48,219,195,1) 95%);
    height: 100%;
  }
  .main-title {
    color: #fff;
    font-size: 40px;
    font-family: 'Poppins' !important;
    text-align: center;
  }
  .loading {
    color:#fff;
    font-size: 18px;
    font-family: 'Poppins';
    text-align: center;   
  }
  .container {
    .row {
      display: flex;
      justify-content: center;
      .main-column {
        text-align: center;
          .secondary-title {
            color: #fff;
            font-size: 30px;
            font-family: 'Poppins' !important;
          }
          .content-text-comic {
            margin-left:20%;
            margin-right:20%;
            line-height:1.8;
          .text-alt,
          .text-comic {
            color:#fff;
            font-size: 16px;
            font-family: 'Comic Neue';           
          }
        }
      }
    }
  }
  
  .btn-random {
    color:#fff;
    font-size: 16px;
    font-family: 'Poppins';
    background: #002551;
    border: solid thin #002551;
    padding: 12px 40px 12px 40px;
    border-radius:15px;
    cursor:pointer;
  }

</style>