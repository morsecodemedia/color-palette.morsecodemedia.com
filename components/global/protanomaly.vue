<template>
  <div
    class="protanomaly"
    :class="{ show: showImpairments }"
  >
    <p class="title">
      Protanomaly
    </p>
    <p class="subtitle">
      Trouble distinguishing reds
    </p>
    <div
      :style="{ backgroundColor: protanomalyBackgroundColor }"
      class="color-box-background"
    >
      <span
        :style="{ color: protanomalyTextColor }"
        class="color-box-text"
      >
        Aa
      </span>
    </div>
    <p>Contrast Ratio: {{ protanomalyContrastRatio }}:1</p>
    <table width="100%">
      <thead>
        <tr>
          <th>&nbsp;</th>
          <th>Normal Text</th>
          <th>Large Text</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>AA</th>
          <td :class="protanomalyAANormal ? 'pass' : 'fail'">
            {{ (protanomalyAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="protanomalyAALarge ? 'pass' : 'fail'">
            {{ (protanomalyAALarge) ? '✓ PASS' : '✗ FAIL' }}
          </td>
        </tr>
        <tr>
          <th>AAA</th>
          <td :class="protanomalyAAANormal ? 'pass' : 'fail'">
            {{ (protanomalyAAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="protanomalyAAALarge ? 'pass' : 'fail'">
            {{ (protanomalyAAALarge) ? '✓ PASS' : '✗ FAIL' }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import blinder from 'color-blind'
import { colord, extend } from 'colord'
import a11yPlugin from 'colord/plugins/a11y'
extend([a11yPlugin])

export default {
  name: 'ProtanomalyCheck',
  props: {
    textColor: {
      type: Object,
      required: true,
      twoWay: true,
      default: () => {}
    },
    backgroundColor: {
      type: Object,
      required: true,
      twoWay: true,
      default: () => {}
    },
    showImpairments: {
      type: Boolean,
      required: true
    }
  },
  data () {
    return {
      protanomalyBackgroundColor: '',
      protanomalyTextColor: '',
      protanomalyContrastRatio: '',
      protanomalyAANormal: false,
      protanomalyAALarge: false,
      protanomalyAAANormal: false,
      protanomalyAAALarge: false
    }
  },
  watch: {
    backgroundColor () {
      this.checkProtanomalyColorContrast(this.textColor.hex, this.backgroundColor.hex)
    },
    textColor () {
      this.checkProtanomalyColorContrast(this.textColor.hex, this.backgroundColor.hex)
    }
  },
  mounted () {
    this.checkProtanomalyColorContrast(this.textColor.hex, this.backgroundColor.hex)
  },
  methods: {
    checkProtanomalyColorContrast (txt, bg) {
      this.protanomalyBackgroundColor = blinder.protanomaly(bg.slice(0, -2))
      this.protanomalyTextColor = blinder.protanomaly(txt.slice(0, -2))
      this.protanomalyContrastRatio = colord(this.protanomalyTextColor).contrast(this.protanomalyBackgroundColor)
      this.protanomalyAANormal = this.protanomalyContrastRatio >= 4.5
      this.protanomalyAALarge = this.protanomalyContrastRatio >= 3
      this.protanomalyAAANormal = this.protanomalyContrastRatio >= 7
      this.protanomalyAAALarge = this.protanomalyContrastRatio >= 4.5
    }
  }
}
</script>

<style lang="scss">
.protanomaly {
  margin: 10px 0;
  display: none;
  &.show {
    display: block;
  }
  .title {
    font-weight: bold;
  }
  .subtitle {
    font-weight: lighter;
  }
  .color-box-background {
    border: 1px solid black;
    height: 50px;
    width: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0 auto;
  }
  td {
    &.pass {
      color: green;
    }
    &.fail {
      color: red;
    }
  }
}
</style>
