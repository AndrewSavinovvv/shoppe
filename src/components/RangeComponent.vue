<template>
  <div
      ref="rootRef"
      class="range-slider"
      tabindex="0"
      aria-valuenow="0"
      :aria-valuemin="props.min"
      :aria-valuemax="props.max"
  >
    <div class="track" :style="trackStyle">
      <div class="filled" :style="filledStyle"></div>
      <div
          class="thumb min-thumb"
          :style="minThumbStyle"
          @mousedown.stop="onThumbMouseDown('min')"
          @touchstart.stop="onThumbMouseDown('min')"
      >
        <span v-if="leftLabelValue">{{ leftLabelValue }}</span>
      </div>
      <div
          class="thumb max-thumb"
          :style="maxThumbStyle"
          @mousedown.stop="onThumbMouseDown('max')"
          @touchstart.stop="onThumbMouseDown('max')"
      >
        <span v-if="rightLabelValue">{{ rightLabelValue }}</span>
      </div>
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
  leftLabelValue: {
    type: [String, Number],
    default: '',
  },
  rightLabelValue: {
    type: [String, Number],
    default: '',
  },
  filledColor: {
    type: String,
    default: '#ddd',
  },
  thumbColor: {
    type: String,
    default: 'blue',
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
  backgroundColor: props.filledColor,
  left: `${minRatio.value}%`,
  width: `${maxRatio.value - minRatio.value}%`,
}));

const minThumbStyle = computed(() => ({
  top: '-4px',
  left: `${minRatio.value}%`,
  position: 'absolute',
  height: '16px',
  width: '16px',
  backgroundColor: props.thumbColor,
  cursor: 'pointer',
  borderRadius: '50%',
}));

const maxThumbStyle = computed(() => ({
  top: '-4px',
  left: `${maxRatio.value}%`,
  position: 'absolute',
  height: '16px',
  width: '16px',
  backgroundColor: props.thumbColor,
  cursor: 'pointer',
  borderRadius: '50%',
}));

const onThumbMouseDown = thumb => {
  activeThumb.value = thumb;
  window.addEventListener('mousemove', onMouseMove);
  window.addEventListener('mouseup', onMouseUp);
  window.addEventListener('touchmove', onMouseMove);
  window.addEventListener('touchend', onMouseUp);
};

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

const onMouseUp = () => {
  window.removeEventListener('mousemove', onMouseMove);
  window.removeEventListener('mouseup', onMouseUp);
  window.removeEventListener('touchmove', onMouseMove);
  window.removeEventListener('touchend', onMouseUp);
  activeThumb.value = null;
};
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
</style>