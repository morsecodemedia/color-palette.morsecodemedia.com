<template>
  <div
    class="tritanomaly"
    :class="{ show: showImpairments }"
  >
    <p class="title">
      Tritanomaly
    </p>
    <p class="subtitle">
      Trouble distinguishing blues
    </p>
    <div
      :style="{ backgroundColor: tritanomalyBackgroundColor }"
      class="color-box-background"
    >
      <span
        :style="{ color: tritanomalyTextColor }"
        class="color-box-text"
      >
        Aa
      </span>
    </div>
    <p>Contrast Ratio: {{ tritanomalyContrastRatio }}:1</p>
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
          <td :class="tritanomalyAANormal ? 'pass' : 'fail'">
            {{ (tritanomalyAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="tritanomalyAALarge ? 'pass' : 'fail'">
            {{ (tritanomalyAALarge) ? '✓ PASS' : '✗ FAIL' }}
          </td>
        </tr>
        <tr>
          <th>AAA</th>
          <td :class="tritanomalyAAANormal ? 'pass' : 'fail'">
            {{ (tritanomalyAAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="tritanomalyAAALarge ? 'pass' : 'fail'">
            {{ (tritanomalyAAALarge) ? '✓ PASS' : '✗ FAIL' }}
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
  name: 'TritanomalyCheck',
  props: {
    textColor: {
      type: String,
      required: true,
      twoWay: true,
      default: ''
    },
    backgroundColor: {
      type: String,
      required: true,
      twoWay: true,
      default: ''
    },
    showImpairments: {
      type: Boolean,
      required: true
    }
  },
  data () {
    return {
      tritanomalyBackgroundColor: '',
      tritanomalyTextColor: '',
      tritanomalyContrastRatio: '',
      tritanomalyAANormal: false,
      tritanomalyAALarge: false,
      tritanomalyAAANormal: false,
      tritanomalyAAALarge: false
    }
  },
  watch: {
    backgroundColor () {
      this.checkTritanomalyColorContrast(this.textColor, this.backgroundColor)
    },
    textColor () {
      this.checkTritanomalyColorContrast(this.textColor, this.backgroundColor)
    }
  },
  mounted () {
    this.checkTritanomalyColorContrast(this.textColor, this.backgroundColor)
  },
  methods: {
    checkTritanomalyColorContrast (txt, bg) {
      this.tritanomalyBackgroundColor = blinder.tritanomaly(bg)
      this.tritanomalyTextColor = blinder.tritanomaly(txt)
      this.tritanomalyContrastRatio = colord(this.tritanomalyTextColor).contrast(this.tritanomalyBackgroundColor)
      this.tritanomalyAANormal = this.tritanomalyContrastRatio >= 4.5
      this.tritanomalyAALarge = this.tritanomalyContrastRatio >= 3
      this.tritanomalyAAANormal = this.tritanomalyContrastRatio >= 7
      this.tritanomalyAAALarge = this.tritanomalyContrastRatio >= 4.5
    }
  }
}
</script>

<style lang="scss">
.tritanomaly {
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
