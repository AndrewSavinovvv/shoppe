<template>
  <div class="search-input">
    <input
        class="shop__left__search"
        placeholder="Search..."
        type="text"
        v-model="searchTerm"
        @input="$emit('update:modelValue', searchTerm)"
    />
  </div>
</template>

<script setup>
import { ref, watch, defineProps, defineEmits } from 'vue';

const props = defineProps({
  modelValue: {
    type: String,
    default: ''
  }
});

const emit = defineEmits();

const searchTerm = ref(props.modelValue);

watch(searchTerm, (newValue) => {
  emit('update:modelValue', newValue);
});
</script>

<style scoped>
.shop__left__search {
  padding: 11px 0;
  border: none;
  border-bottom: 1px solid  #D8D8D8;
  font-size: 14px;
  font-family: "DM Sans", sans-serif;
  color: #707070;
  width: 261px;
}
</style>