<template>
  <div class="bg-white">
    <div>

      <div class="relative z-40 lg:hidden" role="dialog" aria-modal="true">

        <div v-if="showMobileFilter" class="fixed inset-0 bg-black bg-opacity-25"></div>

        <div v-if="showMobileFilter" class="fixed inset-0 z-40 flex">
          <div class="relative ml-auto flex h-full w-full max-w-xs flex-col overflow-y-auto bg-white py-4 pb-12 shadow-xl">
            <div class="flex items-center justify-between px-4">
              <h2 class="text-lg font-medium text-gray-900">Filters</h2>
              <button
                type="button"
                class="-mr-2 flex h-10 w-10 items-center justify-center rounded-md bg-white p-2 text-gray-400"
                @click="showMobileFilter = false"
              >
                <span class="sr-only">Close menu</span>
                <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" aria-hidden="true">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                </svg>
              </button>
            </div>

            <!-- Filters -->
            <form class="mt-4 border-t border-gray-200 px-4 py-6">

              <CountrySelect :countries="countries" @changeCountries="changeCountries"/>
              <ChexpoxesComponent title="Type" :data="types" @changeData="changeType"/>
              <ChexpoxesComponent title="Count of Stars" :data="STARS" @changeData="changeStars"/>
              <div class="border-b border-gray-200 py-6">
                <span class="font-medium text-gray-900">Count of reviews</span>
                <input
                    class="mt-6 block bg-white w-full border border-slate-300 rounded-md py-2 pl-9 pr-3 shadow-sm focus:outline-none focus:border-slate-300 focus:ring-slate-300 focus:ring-1 sm:text-sm"
                    type="number"
                    v-model="filters.numberOfReviews"
                    min="0"
                    step="1" />
              </div>

              <RangeSlider
                  v-if="rerenderSlider"
                  class="mt-6"
                  :minAngle="minMaxPrice.minPrice"
                  :maxAngle="minMaxPrice.maxPrice"
                  @changePrice="changePrice"
              />

              <div>
                <div
                    class="cursor-pointer mt-6 flex w-full justify-center rounded-md bg-violet-700 px-3 py-2 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-violet-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-violet-600"
                    @click="filteredData"
                >
                  Применить Фильтр
                </div>
                <div
                    class="cursor-pointer mt-6 flex w-full border border-gray-200 justify-center rounded-md px-3 py-2 text-sm font-semibold leading-6 text-grey-500 shadow-sm hover:bg-violet-200 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-violet-200"
                    @click="clearFilter"
                >
                  <span>&#9747; </span> Очистить Фильтр
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>

      <main class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div class="flex justify-start">
          <p class="lg:hidden text-indigo-600" v-if="!showMobileFilter" @click="showMobileFilter = !showMobileFilter">Open Filters</p>
        </div>
        <section aria-labelledby="products-heading" class="pb-24 pt-6">
          <div class="grid grid-cols-1 gap-x-8 gap-y-10 lg:grid-cols-4">
            <!-- Filters -->
            <form class="hidden lg:block">
              <h2 class="text-lg font-medium text-gray-900">Filters</h2>
              <CountrySelect :countries="countries" @changeCountries="changeCountries"/>
              <ChexpoxesComponent title="Type" :data="types" @changeData="changeType"/>
              <ChexpoxesComponent title="Count of Stars" :data="STARS" @changeData="changeStars"/>
              <div class="border-b border-gray-200 py-6">
                <span class="font-medium text-gray-900">Count of reviews</span>
                <input
                  class="mt-6 block bg-white w-full border border-slate-300 rounded-md py-2 pl-9 pr-3 shadow-sm focus:outline-none focus:border-slate-300 focus:ring-slate-300 focus:ring-1 sm:text-sm"
                  type="number"
                  v-model="filters.numberOfReviews"
                  min="0"
                  step="1" />
              </div>

              <RangeSlider
                v-if="rerenderSlider"
                class="mt-6"
                :minAngle="minMaxPrice.minPrice"
                :maxAngle="minMaxPrice.maxPrice"
                @changePrice="changePrice"
              />

              <div>
                <div
                  class="cursor-pointer mt-6 flex w-full justify-center rounded-md bg-violet-700 px-3 py-2 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-violet-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-violet-600"
                  @click="filteredData"
                >
                  Применить Фильтр
                </div>
                <div
                  class="cursor-pointer mt-6 flex w-full border border-gray-200 justify-center rounded-md px-3 py-2 text-sm font-semibold leading-6 text-grey-500 shadow-sm hover:bg-violet-200 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-violet-200"
                  @click="clearFilter"
                >
                  <span>&#9747; </span> Очистить Фильтр
                </div>
              </div>
            </form>

            <!-- Product grid -->
            <div class="lg:col-span-3">
              <template v-if="filteredHotelsData.length">
                <HotelComponent class="mb-5" v-for="(data, index) in filteredHotelsData" :hotel="data" :key="index"/>
              </template>
              <template v-else>
                <div class="flex justify-center flex-col items-center">
                  <p class="text-2xl">По данным параметром ничего не найдено</p>
                  <p class="text-sm">Попробуйте изменить параметры фильтрации и вернуться общий каталог</p>
                  <div
                      class=" w-2/5 cursor-pointer bg-violet-200 mt-6 flex w-full border border-gray-200 justify-center rounded-md px-3 py-2 text-sm font-semibold leading-6 text-violet-500 shadow-sm hover:bg-violet-100 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-violet-200"
                      @click="clearFilter"
                  >
                    <span>&#9747; </span> Очистить Фильтр
                  </div>
                </div>
              </template>
            </div>
          </div>
        </section>
      </main>
    </div>
  </div>
</template>

<script lang="ts">
import {ref, reactive, onMounted, watch} from "vue";
import axios from "axios";

import CountrySelect from "@/components/base/CountrySelect.vue";
import ChexpoxesComponent from "@/components/base/ChexpoxesComponent.vue";
import {STARS} from "@/constants/stars";
import RangeSlider from "@/components/base/RangeSlider.vue";
import HotelComponent from "@/components/base/HotelComponent.vue";

// Interface for the hotel data
interface Hotel {
  name: string;
  country: string;
  address: string;
  stars: number;
  type: string;
  description: string;
  services: string[];
  min_price: number;
  currency: string;
  rating: number;
  reviews_amount: number;
  last_review: string;
}
// Interface for the filters
interface Filters {
  countries: string[];
  type: any;
  price: any;
  numberOfReviews: number;
  stars: number[];
}

export default {
  name: "HotelsView",
  components: {HotelComponent, RangeSlider, ChexpoxesComponent, CountrySelect},

  setup() {
    const hotels = ref<Hotel[]>([]);
    const countries = ref<string[]>([]);
    const types = ref<any>([]);
    const isDropdownOpen = ref<boolean>(false);
    const showMobileFilter = ref<boolean>(true);
    const rerenderSlider = ref<boolean>(true);
    const minMaxPrice = ref<any>({});


    let filters: Filters = reactive({
      countries: [],
      type: [],
      price: {
        min: 0,
        max: 0
      },
      numberOfReviews: 0,
      stars: [],
    });

    onMounted(async () => {
      try {
        const res = await axios.get(`/src/data/hotels.json`);
        hotels.value = res.data.hotels as Hotel[];
        buildFilterData()
      } catch (error) {
        console.log(error);
      }
    });

    const buildFilterData = () => {
      countries.value = Array.from(new Set(hotels.value.map((hotel) => hotel.country)));
      types.value = Array.from(new Set(hotels.value.map((hotel) => hotel.type)));
      minMaxPrice.value = hotels.value.reduce(
        (result, obj) => {
          return {
            minPrice: Math.min(result.minPrice, obj.min_price),
            maxPrice: Math.max(result.maxPrice, obj.min_price)
          };
        },
        { minPrice: Infinity, maxPrice: -Infinity }
      );
    }

    const filteredHotelsData = ref<Hotel[]>();
    const filterHotels = () => {
      return hotels.value.filter((hotel) => {
        return (
          (filters.countries.length === 0 || filters.countries.some((item)=>item.includes(hotel.country))) &&
          (filters.type.length === 0 || filters.type.some((item)=>item.includes(hotel.type))) &&
          hotel.min_price >= parseFloat(filters.price.min) &&
          hotel.min_price <= parseFloat(filters.price.max) &&
          hotel.reviews_amount >= filters.numberOfReviews &&
          (filters.stars.length === 0 || filters.stars.includes(hotel.stars))
        );
      });
    };

    watch(filters, () => {
      filteredHotelsData.value = filterHotels();
    }, { immediate: true })

    const changeCountries = (countries) => {
      filters.countries = countries
    }

    const changeStars = (stars) => {
      filters.stars = stars.map(val => parseInt(val));
    }

    const changeType = (type) => {
      filters.type = type
    }

    const changePrice = (price) => {
      filters.price.min = price.min;
      filters.price.max = price.max;
    }
    const filteredData = () => {
      filteredHotelsData.value = filterHotels();
    }

    const clearFilter = () => {
      rerenderSlider.value = false;
      filters.type = [];
      filters.countries = [];
      filters.stars = [];
      filters.price.min = minMaxPrice.value.minPrice;
      filters.price.max = minMaxPrice.value.maxPrice;
      filters.numberOfReviews = 0
      filteredHotelsData.value = filterHotels();
       setTimeout(() => {
         rerenderSlider.value = true;
       }, 0)
    }

    return {
      countries,
      types,
      filters,
      isDropdownOpen,
      minMaxPrice,
      filteredHotelsData,
      changeCountries,
      STARS,
      changeStars,
      changeType,
      changePrice,
      clearFilter,
      rerenderSlider,
      filteredData,
      showMobileFilter
    }
  }
}
</script>

<style scoped>

</style>