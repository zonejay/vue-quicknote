<template>
  <div class="parent">
    <ImageScrollList v-model="selectedImage" :images="images" />
    <select v-model="selectedImage">
      <option v-for="image in images" >{{image}}</option>
    </select>
    {{selectedImage}}
  </div>
</template>

<script setup>
import { ref } from 'vue';
import ImageScrollList from './ImageScrollList.vue';

const images = [
  'image1.jpg',
  'image2.jpg',
  'image3.jpg',
  'image4.jpg',
  'image5.jpg',
  'image6.jpg'
  // 更多图片...
];

const selectedImage = ref('image6.jpg');
</script>
<style>
.parent {
  width: 240px;
}
</style>
