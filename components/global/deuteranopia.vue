<template>
  <div
    class="deuteranopia"
    :class="{ show: showImpairments }"
  >
    <p class="title">
      Deuteranopia
    </p>
    <p class="subtitle">
      Green blind - Can't see greens at all
    </p>
    <div
      :style="{ backgroundColor: deuteranopiaBackgroundColor }"
      class="color-box-background"
    >
      <span
        :style="{ color: deuteranopiaTextColor }"
        class="color-box-text"
      >
        Aa
      </span>
    </div>
    <p>Contrast Ratio: {{ deuteranopiaContrastRatio }}:1</p>
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
          <td :class="deuteranopiaAANormal ? 'pass' : 'fail'">
            {{ (deuteranopiaAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="deuteranopiaAALarge ? 'pass' : 'fail'">
            {{ (deuteranopiaAALarge) ? '✓ PASS' : '✗ FAIL' }}
          </td>
        </tr>
        <tr>
          <th>AAA</th>
          <td :class="deuteranopiaAAANormal ? 'pass' : 'fail'">
            {{ (deuteranopiaAAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="deuteranopiaAAALarge ? 'pass' : 'fail'">
            {{ (deuteranopiaAAALarge) ? '✓ PASS' : '✗ FAIL' }}
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
  name: 'DeuteranopiaCheck',
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
      deuteranopiaBackgroundColor: '',
      deuteranopiaTextColor: '',
      deuteranopiaContrastRatio: '',
      deuteranopiaAANormal: false,
      deuteranopiaAALarge: false,
      deuteranopiaAAANormal: false,
      deuteranopiaAAALarge: false
    }
  },
  watch: {
    backgroundColor () {
      this.checkDeuteranopiaColorContrast(this.textColor, this.backgroundColor)
    },
    textColor () {
      this.checkDeuteranopiaColorContrast(this.textColor, this.backgroundColor)
    }
  },
  mounted () {
    this.checkDeuteranopiaColorContrast(this.textColor, this.backgroundColor)
  },
  methods: {
    checkDeuteranopiaColorContrast (txt, bg) {
      this.deuteranopiaBackgroundColor = blinder.deuteranopia(bg)
      this.deuteranopiaTextColor = blinder.deuteranopia(txt)
      this.deuteranopiaContrastRatio = colord(this.deuteranopiaTextColor).contrast(this.deuteranopiaBackgroundColor)
      this.deuteranopiaAANormal = this.deuteranopiaContrastRatio >= 4.5
      this.deuteranopiaAALarge = this.deuteranopiaContrastRatio >= 3
      this.deuteranopiaAAANormal = this.deuteranopiaContrastRatio >= 7
      this.deuteranopiaAAALarge = this.deuteranopiaContrastRatio >= 4.5
    }
  }
}
</script>

<style lang="scss">
.deuteranopia {
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
