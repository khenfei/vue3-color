<template>
    <div class="vc-twitter"
      :class="{
        'vc-twitter-hide-triangle ': triangle === 'hide',
        'vc-twitter-top-left-triangle ': triangle === 'top-left',
        'vc-twitter-top-right-triangle ': triangle === 'top-right',
      }"
      :style="{
        width: typeof width === 'number' ? `${width}px` : width
      }"
    >
      <div class="vc-twitter-triangle-shadow"></div>
      <div class="vc-twitter-triangle"></div>

      <div class="vc-twitter-body">
        <span
          class="vc-twitter-swatch"
          :style="{
            background: color,
            boxShadow: `0 0 4px ${ equal(color) ? color : 'transparent' }`,
          }"
          v-for="(color, index) in defaultColors"
          :key="index"
          @click="handlerClick(color)"
        >
        </span>
        <div class="vc-twitter-hash">#</div>
        <editable-input label="#" :modelValue="hex" @change="inputChange"></editable-input>
        <div class="vc-twitter-clear"></div>
      </div>
    </div>
</template>

<script>
import editableInput from './common/EditableInput.vue'
import colorMixin from '../mixin/color'

const defaultColors = [
  '#FF6900', '#FCB900', '#7BDCB5', '#00D084', '#8ED1FC', '#0693E3', '#ABB8C3',
  '#EB144C', '#F78DA7', '#9900EF'
]

export default {
  name: 'Twitter',
  mixins: [colorMixin],
  components: {
    editableInput
  },
  props: {
    width: {
      type: [String, Number],
      default: 276
    },
    defaultColors: {
      type: Array,
      default () {
        return defaultColors
      }
    },
    triangle: {
      default: 'top-left',
      validator (value) {
        return ['hide', 'top-left', 'top-right'].includes(value)
      }
    }
  },
  computed: {
    hsv () {
      const hsv = this.colors.hsv
      return {
        h: hsv.h.toFixed(),
        s: (hsv.s * 100).toFixed(),
        v: (hsv.v * 100).toFixed()
      }
    },
    hex () {
      const hex = this.colors.hex
      return hex && hex.replace('#', '')
    }
  },
  methods: {
    equal (color) {
      return color.toLowerCase() === this.colors.hex.toLowerCase()
    },
    handlerClick (color) {
      this.colorChange({
        hex: color,
        source: 'hex'
      })
    },
    inputChange (data) {
      if (!data) {
        return
      }
      if (data['#']) {
        this.isValidHex(data['#']) && this.colorChange({
          hex: data['#'],
          source: 'hex'
        })
      } else if (data.r || data.g || data.b || data.a) {
        this.colorChange({
          r: data.r || this.colors.rgba.r,
          g: data.g || this.colors.rgba.g,
          b: data.b || this.colors.rgba.b,
          a: data.a || this.colors.rgba.a,
          source: 'rgba'
        })
      } else if (data.h || data.s || data.v) {
        this.colorChange({
          h: data.h || this.colors.hsv.h,
          s: (data.s / 100) || this.colors.hsv.s,
          v: (data.v / 100) || this.colors.hsv.v,
          source: 'hsv'
        })
      }
    }
  }
}
</script>
