<template>
  <div
      ref="rootRef"
      class="range-slider"
      tabindex="0"
      aria-valuenow="0"
      :aria-valuemin="props.min"
      :aria-valuemax="props.max"
  >
    <div class="track">
      <div class="filled" :class="colorClass" :style="filledStyle"></div>
      <div
          class="thumb min-thumb"
          :class="colorClass"
          :style="minThumbStyle"
          @mousedown.stop="onThumbMouseDown('min')"
          @touchstart.stop="onThumbMouseDown('min')"
      ></div>

      <div
          class="thumb max-thumb"
          :class="colorClass"
          :style="maxThumbStyle"
          @mousedown.stop="onThumbMouseDown('max')"
          @touchstart.stop="onThumbMouseDown('max')"
      ></div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue';

const props = defineProps({
  modelValue: {
    type: Object,
    default: () => ({ min: 0, max: 100 }),
    validator: value => 'min' in value && 'max' in value,
  },
  min: {
    type: Number,
    default: 0,
  },
  max: {
    type: Number,
    default: 100,
  },
  color: {
    type: String,
    default: 'primary',
  },
});

const emit = defineEmits(['update:modelValue']);

const rootRef = ref(null);
const minRatio = ref(0);
const maxRatio = ref(1);
const activeThumb = ref(null);

watch(
    () => props.modelValue,
    newValue => {
      const minPercent = ((newValue.min - props.min) / (props.max - props.min)) * 100;
      const maxPercent = ((newValue.max - props.min) / (props.max - props.min)) * 100;
      minRatio.value = minPercent;
      maxRatio.value = maxPercent;
    },
    { immediate: true }
);

const trackStyle = computed(() => ({
  width: '100%',
  height: '8px',
  backgroundColor: '#ddd',
  position: 'relative',
}));

const filledStyle = computed(() => ({
  position: 'absolute',
  height: '8px',
  left: `${minRatio.value}%`,
  width: `${maxRatio.value - minRatio.value}%`,
}));

const minThumbStyle = computed(() => ({
  top: '-4px',
  left: `${minRatio.value}%`,
  position: 'absolute',
  height: '16px',
  width: '16px',
  cursor: 'pointer',
  borderRadius: '50%',
}));

const maxThumbStyle = computed(() => ({
  top: '-4px',
  left: `${maxRatio.value}%`,
  position: 'absolute',
  height: '16px',
  width: '16px',
  cursor: 'pointer',
  borderRadius: '50%',
  transform: 'translateX(-50%)',
}));

const onMouseMove = event => {
  const rect = rootRef.value.getBoundingClientRect();
  const x = event.clientX - rect.left;
  const percent = Math.max(0, Math.min(1, x / rect.width));
  const newValue = Math.round(percent * (props.max - props.min) + props.min);

  if (activeThumb.value === 'min') {
    if (newValue >= props.modelValue.max) {
      emit('update:modelValue', { min: props.modelValue.max, max: newValue });
      activeThumb.value = 'max';
    } else {
      emit('update:modelValue', { ...props.modelValue, min: newValue });
    }
  } else if (activeThumb.value === 'max') {
    if (newValue <= props.modelValue.min) {
      emit('update:modelValue', { min: newValue, max: props.modelValue.min });
      activeThumb.value = 'min';
    } else {
      emit('update:modelValue', { ...props.modelValue, max: newValue });
    }
  }
};

let isMouseMoveAdded = false;

const onThumbMouseDown = thumb => {
  activeThumb.value = thumb;
  if (!isMouseMoveAdded) {
    window.addEventListener('mousemove', onMouseMove);
    window.addEventListener('mouseup', onMouseUp);
    window.addEventListener('touchmove', onMouseMove);
    window.addEventListener('touchend', onMouseUp);
    isMouseMoveAdded = true;
  }
};

const onMouseUp = () => {
  window.removeEventListener('mousemove', onMouseMove);
  window.removeEventListener('mouseup', onMouseUp);
  window.removeEventListener('touchmove', onMouseMove);
  window.removeEventListener('touchend', onMouseUp);
  activeThumb.value = null;
  isMouseMoveAdded = false;
};

const colorClass = computed(() => `color-${props.color}`);
</script>


<style scoped>
.range-slider {
  width: 100%;
  user-select: none;
  position: relative;
}

.track {
  position: relative;
  height: 8px;
  background: #ddd;
  border-radius: 10px;
}

.filled {
  position: absolute;
  height: 8px;
  background: black;
}

.thumb {
  position: absolute;
  height: 16px;
  width: 16px;
  border-radius: 50%;
  cursor: pointer;
}

.min-thumb {
  background: blue;
}

.max-thumb {
  background: red;
}
.color-primary {
  background-color: #007bff;
}

.color-orange {
  background-color: #FFA500;
}


.color-green {
  background-color: #28a745;
}

</style>