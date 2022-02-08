<template>
  <div
    class="deuteranomaly"
    :class="{ show: showImpairments }"
  >
    <p class="title">
      Deuteranomaly
    </p>
    <p class="subtitle">
      Trouble distinguishing greens
    </p>
    <div
      :style="{ backgroundColor: deuteranomalyBackgroundColor }"
      class="color-box-background"
    >
      <span
        :style="{ color: deuteranomalyTextColor }"
        class="color-box-text"
      >
        Aa
      </span>
    </div>
    <p>Contrast Ratio: {{ deuteranomalyContrastRatio }}:1</p>
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
          <td :class="deuteranomalyAANormal ? 'pass' : 'fail'">
            {{ (deuteranomalyAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="deuteranomalyAALarge ? 'pass' : 'fail'">
            {{ (deuteranomalyAALarge) ? '✓ PASS' : '✗ FAIL' }}
          </td>
        </tr>
        <tr>
          <th>AAA</th>
          <td :class="deuteranomalyAAANormal ? 'pass' : 'fail'">
            {{ (deuteranomalyAAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="deuteranomalyAAALarge ? 'pass' : 'fail'">
            {{ (deuteranomalyAAALarge) ? '✓ PASS' : '✗ FAIL' }}
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
  name: 'DeuteranomalyCheck',
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
      deuteranomalyBackgroundColor: '',
      deuteranomalyTextColor: '',
      deuteranomalyContrastRatio: '',
      deuteranomalyAANormal: false,
      deuteranomalyAALarge: false,
      deuteranomalyAAANormal: false,
      deuteranomalyAAALarge: false
    }
  },
  watch: {
    backgroundColor () {
      this.checkDeuteranomalyColorContrast(this.textColor.hex, this.backgroundColor.hex)
    },
    textColor () {
      this.checkDeuteranomalyColorContrast(this.textColor.hex, this.backgroundColor.hex)
    }
  },
  mounted () {
    this.checkDeuteranomalyColorContrast(this.textColor.hex, this.backgroundColor.hex)
  },
  methods: {
    checkDeuteranomalyColorContrast (txt, bg) {
      this.deuteranomalyBackgroundColor = blinder.deuteranomaly(bg.slice(0, -2))
      this.deuteranomalyTextColor = blinder.deuteranomaly(txt.slice(0, -2))
      this.deuteranomalyContrastRatio = colord(this.deuteranomalyTextColor).contrast(this.deuteranomalyBackgroundColor)
      this.deuteranomalyAANormal = this.deuteranomalyContrastRatio >= 4.5
      this.deuteranomalyAALarge = this.deuteranomalyContrastRatio >= 3
      this.deuteranomalyAAANormal = this.deuteranomalyContrastRatio >= 7
      this.deuteranomalyAAALarge = this.deuteranomalyContrastRatio >= 4.5
    }
  }
}
</script>

<style lang="scss">
.deuteranomaly {
  display: none;
  margin: 10px 0;
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
