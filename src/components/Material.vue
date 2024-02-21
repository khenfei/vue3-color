<template>
  <div role="application" aria-label="Material color picker" class="vc-material">
    <ed-in class="vc-material-hex" label="hex" v-model="colors.hex"
      :style="{ borderColor: colors.hex }" @change="onChange"></ed-in>

    <div class="vc-material-split">
      <div class="vc-material-third">
        <ed-in label="r" v-model="colors.rgba.r"
        @change="onChange"></ed-in>
      </div>
      <div class="vc-material-third">
        <ed-in label="g" v-model="colors.rgba.g"
        @change="onChange"></ed-in>
      </div>
      <div class="vc-material-third">
        <ed-in label="b" v-model="colors.rgba.b"
        @change="onChange"></ed-in>
      </div>
    </div>
  </div>
</template>

<script>
import editableInput from './common/EditableInput.vue'
import colorMixin from '../mixin/color'

export default {
  name: 'Material',
  mixins: [colorMixin],
  components: {
    'ed-in': editableInput
  },
  methods: {
    onChange (data) {
      if (!data) {
        return
      }
      if (data.hex) {
        this.isValidHex(data.hex) && this.colorChange({
          hex: data.hex,
          source: 'hex'
        })
      } else if (data.r || data.g || data.b) {
        this.colorChange({
          r: data.r || this.colors.rgba.r,
          g: data.g || this.colors.rgba.g,
          b: data.b || this.colors.rgba.b,
          a: data.a || this.colors.rgba.a,
          source: 'rgba'
        })
      }
    }
  }
}
</script>
