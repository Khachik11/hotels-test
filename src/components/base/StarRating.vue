<template>
  <div class="star-rating">
    <span v-for="i in 5" :key="i" @click="rate(i)" class="star" :class="{ 'text-yellow-500': i <= rating, 'gray-star': i > rating }">&#9733;</span>
  </div>
</template>

<script lang="ts">

export default {
  name: 'StarRating',
  props: {
    rating: {
      type: Number,
      required: true
    }
  },
  setup(props: any, {emit}) {

    const rate = (value) => {
      emit('update:rating', value);
    };
    return {rate}
  }
}

</script>

<style>
.star-rating {
  display: inline-block;
}

.star {
  cursor: pointer;
  color: #ddd;
}

.yellow-star {
  color: yellow;
}
</style>
