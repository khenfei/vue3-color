<template>
  <div role="application" aria-label="Compact color picker" class="vc-compact">
    <ul class="vc-compact-colors" role="listbox">
      <li
        v-for="c in paletteUpperCase(palette)"
        role="option"
        :aria-label="'color:' + c"
        :aria-selected="c === pick"
        class="vc-compact-color-item"
        :key="c"
        :class="{'vc-compact-color-item--white': c === '#FFFFFF' }"
        :style="{background: c}"
        @click="handlerClick(c)"
      >
        <div class="vc-compact-dot" v-show="c === pick"></div>
      </li>
    </ul>
  </div>
</template>

<script>
import colorMixin from '../mixin/color'
// import editableInput from './common/EditableInput.vue'

const defaultColors = [
  '#4D4D4D', '#999999', '#FFFFFF', '#F44E3B', '#FE9200', '#FCDC00',
  '#DBDF00', '#A4DD00', '#68CCCA', '#73D8FF', '#AEA1FF', '#FDA1FF',
  '#333333', '#808080', '#CCCCCC', '#D33115', '#E27300', '#FCC400',
  '#B0BC00', '#68BC00', '#16A5A5', '#009CE0', '#7B64FF', '#FA28FF',
  '#000000', '#666666', '#B3B3B3', '#9F0500', '#C45100', '#FB9E00',
  '#808900', '#194D33', '#0C797D', '#0062B1', '#653294', '#AB149E'
]

export default {
  name: 'Compact',
  mixins: [colorMixin],
  props: {
    palette: {
      type: Array,
      default () {
        return defaultColors
      }
    }
  },
  components: {
    // 'ed-in': editableInput
  },
  computed: {
    pick () {
      return this.colors.hex.toUpperCase()
    }
  },
  methods: {
    handlerClick (c) {
      this.colorChange({
        hex: c,
        source: 'hex'
      })
    }
  }
}

</script>