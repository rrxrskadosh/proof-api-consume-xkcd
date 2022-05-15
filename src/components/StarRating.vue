<template>
  <p class="text-rating">You can give it the rating!</p>
  <div class="container">
    <div class="star-rating">
      <label class="star-rating__star" v-for="rating in ratings"
      :class="{
      'is-selected': ((value >= rating) && value != null), 'is-disabled': disabled}" 
      v-on:click="set(rating)" v-on:mouseover="star_over(rating)" v-on:mouseout="star_out">
      <input class="star-rating star-rating__checkbox" type="radio" :value="rating" :name="name"
      v-model="value" :disabled="disabled">★</label>
    </div>
  </div>
</template>


<script>

export default {
  name: 'StarRating',
  components: {},
  props: {
    'name': String,
    'value': null,
    'id': String,
    'disabled': Boolean,
    'required': Boolean
  },
  data: function() {
    return {
      temp_value: null,
      ratings: [1, 2, 3, 4, 5]
    };
  },

  methods: {
    /* Behaviour of the stars on mouseover */
    star_over: function(index) {
      var self = this;

      if (!this.disabled) {
        this.temp_value = this.value;
        return this.value = index;
      }

    },

    /* Behaviour of the stars on mouseout */
    star_out: function() {
      var self = this;

      if (!this.disabled) {
        return this.value = this.temp_value;
      }
    },

    /* Set the rating. */
    set: function(value) {
      var self = this;

      if (!this.disabled) {
        this.temp_value = value;
        return this.value = value;
      }
    }
  }
}

</script>

<style lang="scss" scoped>

%visually-hidden {
  position: absolute;
  overflow: hidden;
  clip: rect(0 0 0 0);
  height: 1px; width: 1px;
  margin: -1px; padding: 0; border: 0;
}
.text-rating {
  color:#fff;
  font-size:18px;
  font-family: 'Poppins';
}
.container {
  display:flex;
  justify-content: center;
  align-items:center;
  margin-bottom: 20px;
  .star-rating {
  &__star {
    display: inline-block;
    padding: 5px;
    vertical-align: middle;
    line-height: 1;
    font-size: 3em;
    color: #fff;
    transition: color .2s ease-out;

    &:hover {
      cursor: pointer;
    }
    
    &.is-selected {
      color: #FFD700;
    }
    
    &.is-disabled:hover {
      cursor: default;
    }
  }

  &__checkbox {
    @extend %visually-hidden;
  }
}
}

</style>
