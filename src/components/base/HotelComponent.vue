<template>
  <ul role="list" class="divide-y divide-gray-100">
    <li class=" p-5 border-gray-200 border rounded-md">
      <div class="flex w-full justify-between mt-4">
        <p class="text-xl font-bold leading-6 text-gray-900">{{ hotel.name }}</p>
        <p class="text-xl font-bold leading-6 text-gray-900">{{ hotel.min_price }} {{ hotel.currency }}</p>
      </div>
      <div class="flex w-full justify-between mt-4">
        <div class="flex items-center flex-wrap">
          <StarRating :rating="hotel.stars" />
          <p class="ml-3 text-xs text-gray-400">{{ hotel.type }}</p>
          <p class="ml-3 text-xs text-gray-400">{{ hotel.reviews_amount }} reviews</p>
          <p class="ml-3 text-xs text-gray-400">&#9737; {{ hotel.country }}</p>
        </div>
        <div class="flex items-center">
          <p class="ml-3 text-xs text-gray-400">цена за 1 ночь</p>
        </div>
      </div>
      <div class="flex w-full justify-between mt-4 flex-wrap">
        <div class="flex items-center w-4/5">
          <p class="text-sm text-gray-600">{{ hotel.description }}</p>
        </div>
        <div class="flex items-center w-1/5 min-w-min mt-4">
          <BookComponent :book="hotel.stars > 4"/>
        </div>
      </div>
    </li>
  </ul>

</template>

<script>
import StarRating from "@/components/base/StarRating.vue";
import BookComponent from "@/components/base/BookComponent.vue";
export default {
  name: "HotelComponent",
  components: {BookComponent, StarRating},
  props: {
    hotel: {
      type: Object,
      default: () => {}
    }
  }
}
</script>

<style scoped>

</style>