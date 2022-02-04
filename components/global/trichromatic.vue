<template>
  <div class="trichromatic show">
    <p class="title">
      Regular Vision (Trichromatic)
    </p>
    <p class="subtitle">
      Can distinguish all three primary colors, little to no blurriness
    </p>
    <div :style="{ backgroundColor: backgroundColor.hex }" class="color-box-background">
      <span class="color-box-text" :style="{ color: textColor.hex }">
        Aa
      </span>
    </div>
    <p>Contrast Ratio: {{ contrastRatio }}:1</p>
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
          <td :class="aaNormal ? 'pass' : 'fail'">
            {{ (aaNormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="aaLarge ? 'pass' : 'fail'">
            {{ (aaLarge) ? '✓ PASS' : '✗ FAIL' }}
          </td>
        </tr>
        <tr>
          <th>AAA</th>
          <td :class="aaaNormal ? 'pass' : 'fail'">
            {{ (aaaNormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="aaaLarge ? 'pass' : 'fail'">
            {{ (aaaLarge) ? '✓ PASS' : '✗ FAIL' }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { colord, extend } from 'colord'
import a11yPlugin from 'colord/plugins/a11y'
extend([a11yPlugin])

export default {
  name: 'TrichromaticCheck',
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
    }
  },
  data () {
    return {
      contrastRatio: '',
      aaNormal: false,
      aaLarge: false,
      aaaNormal: false,
      aaaLarge: false
    }
  },
  watch: {
    backgroundColor () {
      this.checkColorContrast(this.textColor.hex, this.backgroundColor.hex)
    },
    textColor () {
      this.checkColorContrast(this.textColor.hex, this.backgroundColor.hex)
    }
  },
  mounted () {
    this.checkColorContrast(this.textColor.hex, this.backgroundColor.hex)
  },
  methods: {
    checkColorContrast (txt, bg) {
      this.contrastRatio = colord(txt.slice(0, -2)).contrast(bg.slice(0, -2))
      this.aaNormal = this.contrastRatio >= 4.5
      this.aaLarge = this.contrastRatio >= 3
      this.aaaNormal = this.contrastRatio >= 7
      this.aaaLarge = this.contrastRatio >= 4.5
    }
  }
}
</script>

<style lang="scss">
.trichromatic {
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
