<template>
  <div role="application" aria-label="PhotoShop color picker" :class="['vc-photoshop', disableFields ? 'vc-photoshop__disable-fields' : '']">
    <div role="heading" class="vc-ps-head">{{head}}</div>
    <div class="vc-ps-body">
      <div class="vc-ps-saturation-wrap">
        <saturation v-model="colors" @change="childChange"></saturation>
      </div>
      <div class="vc-ps-hue-wrap">
        <hue v-model="colors" @change="childChange" direction="vertical">
          <div class="vc-ps-hue-pointer">
            <i class="vc-ps-hue-pointer--left"></i><i class="vc-ps-hue-pointer--right"></i>
          </div>
        </hue>
      </div>
      <div :class="['vc-ps-controls', disableFields ? 'vc-ps-controls__disable-fields' : '']">
        <div class="vc-ps-previews">
          <div class="vc-ps-previews__label">{{ newLabel }}</div>
          <div class="vc-ps-previews__swatches">
            <div class="vc-ps-previews__pr-color" :aria-label="`New color is ${colors.hex}`" :style="{background: colors.hex}"></div>
            <div class="vc-ps-previews__pr-color" :aria-label="`Current color is ${currentColor}`" :style="{background: currentColor}" @click="clickCurrentColor"></div>
          </div>
          <div class="vc-ps-previews__label">{{ currentLabel }}</div>
        </div>
        <div class="vc-ps-actions" v-if="!disableFields">
          <div class="vc-ps-ac-btn" role="button" :aria-label="acceptLabel" @click="handleAccept">{{ acceptLabel }}</div>
          <div class="vc-ps-ac-btn" role="button" :aria-label="cancelLabel" @click="handleCancel">{{ cancelLabel }}</div>

          <div class="vc-ps-fields">
            <!-- hsla -->
            <ed-in label="h" desc="°" :modelValue="hsv.h" @change="inputChange"></ed-in>
            <ed-in label="s" desc="%" :modelValue="hsv.s" :max="100" @change="inputChange"></ed-in>
            <ed-in label="v" desc="%" :modelValue="hsv.v" :max="100" @change="inputChange"></ed-in>
            <div class="vc-ps-fields__divider"></div>
            <!-- rgba -->
            <ed-in label="r" :modelValue="colors.rgba.r" @change="inputChange"></ed-in>
            <ed-in label="g" :modelValue="colors.rgba.g" @change="inputChange"></ed-in>
            <ed-in label="b" :modelValue="colors.rgba.b" @change="inputChange"></ed-in>
            <div class="vc-ps-fields__divider"></div>
            <!-- hex -->
            <ed-in label="#" class="vc-ps-fields__hex" :modelValue="hex" @change="inputChange"></ed-in>
          </div>

          <div v-if="hasResetButton" class="vc-ps-ac-btn" aria-label="reset" @click="handleReset">{{ resetLabel }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import colorMixin from '../mixin/color'
import editableInput from './common/EditableInput.vue'
import saturation from './common/Saturation.vue'
import hue from './common/Hue.vue'
// import alpha from './common/Alpha.vue'

export default {
  name: 'Photoshop',
  mixins: [colorMixin],
  props: {
    head: {
      type: String,
      default: 'Color Picker'
    },
    disableFields: {
      type: Boolean,
      default: false
    },
    hasResetButton: {
      type: Boolean,
      default: false
    },
    acceptLabel: {
      type: String,
      default: 'OK'
    },
    cancelLabel: {
      type: String,
      default: 'Cancel'
    },
    resetLabel: {
      type: String,
      default: 'Reset'
    },
    newLabel: {
      type: String,
      default: 'new'
    },
    currentLabel: {
      type: String,
      default: 'current'
    }
  },
  components: {
    saturation,
    hue,
    // alpha,
    'ed-in': editableInput
  },
  data () {
    return {
      currentColor: '#FFF'
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
  created () {
    this.currentColor = this.colors.hex
  },
  methods: {
    childChange (data) {
      this.colorChange(data)
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
    },
    clickCurrentColor () {
      this.colorChange({
        hex: this.currentColor,
        source: 'hex'
      })
    },
    handleAccept () {
      this.$emit('ok')
    },
    handleCancel () {
      this.$emit('cancel')
    },
    handleReset () {
      this.$emit('reset')
    }
  }

}
</script>
