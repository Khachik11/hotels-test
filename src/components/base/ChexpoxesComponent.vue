<template>
  <div class="border-b border-gray-200 py-6">
    <h3 class="-my-3 flow-root">
      <button type="button" class="flex w-full items-center justify-between bg-white py-3 text-sm text-gray-400 hover:text-gray-500" aria-controls="filter-section-0" aria-expanded="false">
        <span class="font-medium text-gray-900">{{title}}</span>
        <span class="ml-6 flex items-center">
        <svg v-if="!showCheckboxList" @click="showCheckboxList = !showCheckboxList" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
          <path d="M10.75 4.75a.75.75 0 00-1.5 0v4.5h-4.5a.75.75 0 000 1.5h4.5v4.5a.75.75 0 001.5 0v-4.5h4.5a.75.75 0 000-1.5h-4.5v-4.5z" />
        </svg>
        <svg v-else @click="showCheckboxList = !showCheckboxList" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
          <path fill-rule="evenodd" d="M4 10a.75.75 0 01.75-.75h10.5a.75.75 0 010 1.5H4.75A.75.75 0 014 10z" clip-rule="evenodd" />
        </svg>
      </span>
      </button>
    </h3>
    <!-- Filter section, show/hide based on section state. -->
    <div class="pt-6" id="filter-section-0" v-if="showCheckboxList">
      <div class="space-y-4">
        <div class="flex items-center" v-for="(data, index) in checkboxData" :key="data.value">
          <input
              :id="`${title}-filter-${index}`"
              :value="data.value"
              type="checkbox"
              class="cursor-pointer h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500"
              @change="changeData(data)"
          >
          <label :for="`${title}-filter-${index}`" class="cursor-pointer ml-3 text-sm text-gray-600">{{data.value}}</label>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import {computed, ref} from "vue";
interface Checkbox {
  value: string;
  checked: boolean;
}
export default {
  name: "ChexpoxesComponent",
  props: {
    title: {
      type: String,
      required: true
    },
    data: {
      type: Array,
      required: true
    }
  },
  setup(props: any, {emit}) {
    const showCheckboxList = ref<boolean>(false);
    const selectedData = ref<string[]>([]);

    const checkboxData = computed<Checkbox[]>(() =>
        props.data?.map((item) => ({ value: item, checked: false }))
    );

    const changeData = (data) => {
      data.checked = !data.checked;
      const index = selectedData.value.indexOf(data.value);
      if (index !== -1) {
        selectedData.value.splice(index, 1);
      } else {
        selectedData.value.push(data.value);
      }
      emit('changeData', selectedData.value);
    }

    console.log(checkboxData)

    return {
      checkboxData,
      showCheckboxList,
      changeData
    }
  }
}
</script>

<style scoped>

</style>