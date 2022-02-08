<template>
  <div
    class="achromatopsia"
    :class="{ show: showImpairments }"
  >
    <p class="title">
      Achromatopsia
    </p>
    <p class="subtitle">
      Complete color blindness, can only see shades
    </p>
    <div
      :style="{ backgroundColor: achromatopsiaBackgroundColor }"
      class="color-box-background"
    >
      <span
        :style="{ color: achromatopsiaTextColor }"
        class="color-box-text"
      >
        Aa
      </span>
    </div>
    <p>Contrast Ratio: {{ achromatopsiaContrastRatio }}:1</p>
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
          <td :class="achromatopsiaAANormal ? 'pass' : 'fail'">
            {{ (achromatopsiaAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="achromatopsiaAALarge ? 'pass' : 'fail'">
            {{ (achromatopsiaAALarge) ? '✓ PASS' : '✗ FAIL' }}
          </td>
        </tr>
        <tr>
          <th>AAA</th>
          <td :class="achromatopsiaAAANormal ? 'pass' : 'fail'">
            {{ (achromatopsiaAAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="achromatopsiaAAALarge ? 'pass' : 'fail'">
            {{ (achromatopsiaAAALarge) ? '✓ PASS' : '✗ FAIL' }}
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
  name: 'AchromatopsiaCheck',
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
      achromatopsiaBackgroundColor: '',
      achromatopsiaTextColor: '',
      achromatopsiaContrastRatio: '',
      achromatopsiaAANormal: false,
      achromatopsiaAALarge: false,
      achromatopsiaAAANormal: false,
      achromatopsiaAAALarge: false
    }
  },
  watch: {
    backgroundColor () {
      this.checkAchromatopsiaColorContrast(this.textColor.hex, this.backgroundColor.hex)
    },
    textColor () {
      this.checkAchromatopsiaColorContrast(this.textColor.hex, this.backgroundColor.hex)
    }
  },
  mounted () {
    this.checkAchromatopsiaColorContrast(this.textColor.hex, this.backgroundColor.hex)
  },
  methods: {
    checkAchromatopsiaColorContrast (txt, bg) {
      this.achromatopsiaBackgroundColor = blinder.achromatopsia(bg.slice(0, -2))
      this.achromatopsiaTextColor = blinder.achromatopsia(txt.slice(0, -2))
      this.achromatopsiaContrastRatio = colord(this.achromatopsiaTextColor).contrast(this.achromatopsiaBackgroundColor)
      this.achromatopsiaAANormal = this.achromatopsiaContrastRatio >= 4.5
      this.achromatopsiaAALarge = this.achromatopsiaContrastRatio >= 3
      this.achromatopsiaAAANormal = this.achromatopsiaContrastRatio >= 7
      this.achromatopsiaAAALarge = this.achromatopsiaContrastRatio >= 4.5
    }
  }
}
</script>

<style lang="scss">
.achromatopsia {
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
