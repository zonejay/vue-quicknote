<template>
  <div
    class="horizontal-scroll" 
    ref="scrollContainer"
    @scroll="handleScroll"
    @wheel="handleWheel"
    @keydown.shift="handleShiftKeyDown"
    @keyup.shift="handleShiftKeyUp"
  >
    <div
      class="image-item"
      v-for="(image, index) in images"
      :key="index"
      :class="{ selected: modelValue === image }"
      @click="selectImage(image)"
    >
      {{image}}
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, nextTick } from 'vue';
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  modelValue: String,
  images: Array,
});

const emit = defineEmits(['update:modelValue']);
const scrollContainer = ref(null);
let shouldScroll = true;
let shiftKeyPressed = false;

const selectImage = (image) => {
  shouldScroll = true;
  emit('update:modelValue', image);
};

const handleScroll = () => {
  shouldScroll = false;
};
function handleWheel(event) {
  console.log(event)
  if (shiftKeyPressed) return; // 如果按下 Shift 键，则不处理滚轮事件
  event.preventDefault();
  const container = scrollContainer.value;
  // 使用 deltaX 和 deltaY 进行滚动
  if (event.deltaY !== 0) {
    const delta = Math.max(-1, Math.min(1, event.deltaY));
    container.scrollLeft += delta * 10; // 调整滚动速度
  } else if (event.deltaX !== 0) {
    const delta = Math.max(-1, Math.min(1, event.deltaX));
    container.scrollLeft += delta * 10; // 调整滚动速度
  }
}

const scrollToSelectedImage = () => {
  // if (!shouldScroll) return;
  console.log('sb')
  const selectedImage = scrollContainer.value.querySelector('.selected');
  console.log(selectedImage)

  if (selectedImage) {
    selectedImage.scrollIntoView({inline: 'center', behavior: 'smooth'});
  }
};
const handleShiftKeyDown = () => {
  shiftKeyPressed = true;
};

const handleShiftKeyUp = () => {
  shiftKeyPressed = false;
};

onMounted(() => {
  scrollToSelectedImage();
});

watch(() => props.modelValue, () => {
  nextTick(() => {
    scrollToSelectedImage();
  })
});
</script>

<style scoped>
.horizontal-scroll {
  overflow-x: auto;
  white-space: nowrap;
  width: 100%;
  display: inline-flex;
}

.scroll-content {
  display: inline-flex;
}

.image-item {
  flex: 0 0 auto;
  width: 80px;
  height: 80px;
  margin-right: 10px;
  cursor: pointer;
  border: 2px solid transparent;
  background-color: aqua;
}

.image-item.selected {
  border-color: blue;
}

.image-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
