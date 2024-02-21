<template>
  <div role="application" aria-label="Slider color picker" class="vc-slider">
    <div class="vc-slider-hue-warp">
      <hue v-model="colors" @change="hueChange"></hue>
    </div>
    <div class="vc-slider-swatches" role="group">
      <div class="vc-slider-swatch" v-for="(swatch, index) in normalizedSwatches" :key="index" :data-index="index"
        :aria-label="'color:' + colors.hex"
        role="button"
        @click="handleSwClick(index, swatch)">
        <div
          class="vc-slider-swatch-picker"
          :class="{'vc-slider-swatch-picker--active': isActive(swatch, index), 'vc-slider-swatch-picker--white': swatch.l === 1}"
          :style="{background: 'hsl(' + colors.hsl.h + ', ' + swatch.s * 100 + '%, ' + swatch.l * 100 + '%)'}"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
import colorMixin from '../mixin/color'
import hue from './common/Hue.vue'

const DEFAULT_SATURATION = 0.5

export default {
  name: 'Slider',
  mixins: [colorMixin],
  props: {
    swatches: {
      type: Array,
      default () {
        // also accepts: ['.80', '.65', '.50', '.35', '.20']
        return [
          { s: DEFAULT_SATURATION, l: 0.8 },
          { s: DEFAULT_SATURATION, l: 0.65 },
          { s: DEFAULT_SATURATION, l: 0.5 },
          { s: DEFAULT_SATURATION, l: 0.35 },
          { s: DEFAULT_SATURATION, l: 0.2 }
        ]
      }
    }
  },
  components: {
    hue
  },
  computed: {
    normalizedSwatches () {
      const swatches = this.swatches
      return swatches.map(swatch => {
        // to be compatible with another data format ['.80', '.65', '.50', '.35', '.20']
        if (typeof swatch !== 'object') {
          return {
            s: DEFAULT_SATURATION,
            l: swatch
          }
        }
        return swatch
      })
    }
  },
  methods: {
    isActive (swatch, index) {
      const hsl = this.colors.hsl
      if (hsl.l === 1 && swatch.l === 1) {
        return true
      }
      if (hsl.l === 0 && swatch.l === 0) {
        return true
      }
      return (
        Math.abs(hsl.l - swatch.l) < 0.01 && Math.abs(hsl.s - swatch.s) < 0.01
      )
    },
    hueChange (data) {
      this.colorChange(data)
    },
    handleSwClick (index, swatch) {
      this.colorChange({
        h: this.colors.hsl.h,
        s: swatch.s,
        l: swatch.l,
        source: 'hsl'
      })
    }
  }
}
</script>
