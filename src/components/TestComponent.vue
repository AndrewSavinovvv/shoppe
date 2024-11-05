<template>
  <div>
    <div class="test">
      Model : {{rangeValues.min}} to {{rangeValues.max}}
    </div>
    <RangeComponent
        v-model="rangeValues"
        :min="0"
        :max="100"
        color= "orange"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import RangeComponent from '@/components/RangeComponent.vue'

const rangeValues = ref({
  min: 0,
  max: 100
})
</script>

<style scoped>
.test {
  margin-bottom: 20px;
}

</style>