<template>
  <div id="app">
    <div class="header-container">
      <div class="header">
        <div class="intro-wrap">
          <div class="intro">
            <h1>Vue3-color</h1>
            <p role="presentation">A Collection of Color Pickers from Sketch, Photoshop, Chrome, Github, Twitter, Material
              Design & more</p>
            <br />
            <p role="presentation">This package bring Vue 3 support to original package : </p>
            <br />
            <a class="vue-color" href="https://www.npmjs.com/package/vue-color" title="vue-color">vue-color</a>
          </div>
        </div>
        <div class="header-bg" :style="{ 'background-color': bgc }"></div>
        <div class="demo-item">
          <Chrome :modelValue="colors" @update:model-value="updateValue"></Chrome>
          <h2>Chrome</h2>
        </div>
      </div>
    </div>

    <div class="demo-container">
      <div class="demo-list">
        <div class="demo-item">
          <Sketch v-model="colors"></Sketch>
          <h2>Sketch</h2>
        </div>
        <div class="demo-item">
          <Photoshop v-model="colors" @ok="onOk" @cancel="onCancel"></Photoshop>
          <h2>Photoshop</h2>
        </div>
      </div>
      <div class="demo-list">
        <div class="demo-item">
          <Material v-model="colors"></Material>
          <h2>Material</h2>
        </div>
        <div class="demo-item">
          <Slider v-model="colors"></Slider>
          <h2>Slider</h2>
        </div>
      </div>
      <div class="demo-list">
        <div class="demo-item">
          <Compact v-model="colors"></Compact>
          <h2>Compact</h2>
          <br>
          <br>
          <Grayscale :modelValue="colors" @update:model-value="updateValue"></Grayscale>
          <h2>Grayscale</h2>
        </div>
        <div class="demo-item">
          <Swatches v-model="colors"></Swatches>
          <h2>Swatches</h2>
        </div>
      </div>
      <div class="demo-list">
        <div class="demo-item">
          <Twitter :modelValue="colors" @update:model-value="updateValue"></Twitter>
          <h2>Twitter</h2>
        </div>
      </div>

    </div>
  </div>
</template>
<script setup>
import { ref, computed } from 'vue';
import { 
  Material, 
  Compact, 
  Grayscale, 
  Swatches, 
  Slider, 
  Sketch, 
  Chrome,
  Photoshop,
  Twitter
 } from './build.js';

const colors = ref({
  hex: '#194d33e6',
  hsl: {
    h: 150,
    s: 0.5,
    l: 0.2,
    a: 0.9
  },
  hsv: {
    h: 150,
    s: 0.66,
    v: 0.30,
    a: 0.9
  },
  rgba: {
    r: 159,
    g: 96,
    b: 43,
    a: 0.9
  },
  a: 0.9
});

const bgc = computed(() => {
  return colors.value.hex;
});

const onOk = () => {
  console.log('ok');
}

const onCancel = () => {
  console.log('cancel');
}

const updateValue = (value) => {
  colors.value = value;
}
</script>