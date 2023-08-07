<template>
  <div>
    <div class="h-16 relative text-center m-auto flex justify-between" v-if="maxAngle && minAngle">
      <input class="absolute bottom-0 left-0 w-full" type="range" :min="minAngle" :max="maxAngle" step="1" v-model="minAngleValue">
      <input class="border border-slate-200 text-2xl text-center h-10 p-3 rounded" type="number" :min="minAngle" :max="maxAngle" step="1" v-model="minAngleValue">
      <input class="absolute bottom-0 left-0 w-full" type="range" :min="minAngle" :max="maxAngle" step="1" v-model="maxAngleValue">
      <input class="border border-slate-200 text-2xl text-center h-10 p-3 rounded" type="number" :min="minAngle" :max="maxAngle" step="1" v-model="maxAngleValue">
    </div>
  </div>
</template>

<script lang="ts">
import {ref, watch} from 'vue';
export default {
  name: "RangeSlider",
  props: {
    minAngle: {
      type: Number,
      required: true
    },
    maxAngle: {
      type: Number,
      required: true
    }
  },
  setup(props: any, { emit }) {
    const maxAngleValue = ref<string>();
    const minAngleValue = ref<string>();

    watch(() => [props.maxAngle, props.minAngle], () => {
      maxAngleValue.value = props.maxAngle?.toString();
      minAngleValue.value = props.minAngle?.toString();
    }, { immediate: true })

    watch([maxAngleValue, minAngleValue], ([newVal1, newVal2]) => {
      setTimeout(() => {
        emit('changePrice', { max: newVal1, min: newVal2 });
      }, 300)
    }, { immediate: true });

    return {
      maxAngleValue,
      minAngleValue,
    };
  },
}
</script>

<style scoped>
input[type=number]::-webkit-outer-spin-button,
input[type=number]::-webkit-inner-spin-button {
  -webkit-appearance: none;
}



input[type=range] {
  -webkit-appearance: none;
}

input[type=range]:focus {
  outline: none;
}

input[type=range]:focus::-webkit-slider-runnable-track {
  background: #24e334;
}

input[type=range]:focus::-ms-fill-lower {
  background: #24e334;
}

input[type=range]:focus::-ms-fill-upper {
  background: #24e334;
}

input[type=range]::-webkit-slider-runnable-track {
  width: 100%;
  height: 5px;
  cursor: pointer;
  animate: 0.2s;
  background: #24e334;
  border-radius: 1px;
  box-shadow: none;
  border: 0;
}

input[type=range]::-webkit-slider-thumb {
  z-index: 2;
  position: relative;
  box-shadow: 0px 0px 0px #000;
  border: 1px solid #24e334;
  height: 18px;
  width: 18px;
  border-radius: 25px;
  background: white;
  cursor: pointer;
  -webkit-appearance: none;
  margin-top: -7px;
}

</style>