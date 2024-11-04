<template>
  <div>
    <h1>demo</h1>
    <RangeComponent
        v-model="rangeValues"
        :min="0"
        :max="100"
        :left-label-value="rangeValues.min"
        :right-label-value="rangeValues.max"
    />
    <div>
      <p>min: {{ rangeValues.min }}</p>
      <p>max: {{ rangeValues.max }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import RangeComponent from '@/components/RangeComponent.vue'

const rangeValues = ref({
  min: 20,
  max: 80
})
</script>

<style scoped>
h1 {
  margin-bottom: 20px;
}
</style>