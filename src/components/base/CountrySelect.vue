<template>
  <div>
    <label class="block text-sm font-medium leading-6 text-gray-900">Countries</label>
    <div class="relative mt-2">
      <button type="button" @click="showCountriesList = true" class="relative w-full cursor-default rounded-md bg-white py-1.5 pl-3 pr-10 text-left text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 sm:text-sm sm:leading-6" aria-haspopup="listbox" aria-expanded="true" aria-labelledby="listbox-label">
      <span class="flex items-center h-5">
        <span class="ml-3 block truncate">{{ selectedCountries.length ? `Selected ${selectedCountries.length}` : 'Choose Country' }}</span>
      </span>
        <span class="pointer-events-none absolute inset-y-0 right-0 ml-3 flex items-center pr-2">
        <svg class="h-5 w-5 text-gray-400" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
          <path fill-rule="evenodd" d="M10 3a.75.75 0 01.55.24l3.25 3.5a.75.75 0 11-1.1 1.02L10 4.852 7.3 7.76a.75.75 0 01-1.1-1.02l3.25-3.5A.75.75 0 0110 3zm-3.76 9.2a.75.75 0 011.06.04l2.7 2.908 2.7-2.908a.75.75 0 111.1 1.02l-3.25 3.5a.75.75 0 01-1.1 0l-3.25-3.5a.75.75 0 01.04-1.06z" clip-rule="evenodd" />
        </svg>
      </span>
      </button>
      <ul v-if="showCountriesList" class="absolute z-10 mt-1 max-h-56 w-full overflow-auto rounded-md bg-white py-1 text-base shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none sm:text-sm" tabindex="-1" role="listbox" aria-labelledby="listbox-label" aria-activedescendant="listbox-option-3">
        <li class="text-gray-900 relative cursor-default select-none py-2 pl-3 pr-9" id="listbox-option-0" role="option">
          <div class="flex items-center" v-for="country in countriesData" :key="country.value">
            <span class="text-indigo-600 absolute inset-y-0 left-2 flex items-center pr-4">
              <input :checked="country.checked" @change="chooseCountry(country)"  name="countries[]" value="white" type="checkbox" class="cursor-pointer h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500">
            </span>
            <span class="cursor-pointer font-normal ml-6 block truncate" @click="chooseCountry(country)">{{ country.value }}</span>
          </div>
        </li>
      </ul>
    </div>
  </div>

</template>

<script lang="ts">
import {computed, ref} from "vue";

interface Country {
  value: string;
  checked: boolean;
}

export default {
  name: "CountrySelect",
  props: {
    countries: {
      type: Array,
      default: () => []
    }
  },
  setup(props: any, { emit }) {
    const selectedCountries = ref<string[]>([]);
    const showCountriesList = ref<boolean>(false);

    const countriesData = computed<Country[]>(() => props.countries?.map((item) => ({ value: item, checked: false })));
    const chooseCountry = (country) => {
      country.checked = !country.checked;
      const index = selectedCountries.value.indexOf(country.value);
      if (index !== -1) {
        selectedCountries.value.splice(index, 1);
      } else {
        selectedCountries.value.push(country.value);
      }
      emit('changeCountries', selectedCountries.value)
      showCountriesList.value = false;
    }

    return {
      countriesData,
      selectedCountries,
      showCountriesList,
      chooseCountry
    }

  }
}
</script>

<style scoped>

</style>