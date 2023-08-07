<template>
<div class="w-full flex justify-center rounded-md items-center p-2"
     :class="[book ? 'bg-green-100' : 'bg-purple-100']"
>
  <span v-if="book" class="text-green-600">&#9745;</span>
  <span v-else class="text-purple-600">&#9781;</span>
  <p class="text-sm ml-1" :class="[book ? 'text-green-600' : 'tex-purple-600']">{{ book ? 'Забронировано' : 'Забронировать '}}</p>
</div>
</template>

<script>
export default {
  name: "BookComponent",
  props: {
    book: {
      type: Boolean,
      default: true
    }
  }
}
</script>

<style scoped>

</style>