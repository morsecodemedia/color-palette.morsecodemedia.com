<template>
  <div
    class="protanopia"
    :class="{ show: showImpairments }"
  >
    <p class="title">
      Protanopia
    </p>
    <p class="subtitle">
      Red blind - Can't see reds at all
    </p>
    <div
      :style="{ backgroundColor: protanopiaBackgroundColor }"
      class="color-box-background"
    >
      <span
        :style="{ color: protanopiaTextColor }"
        class="color-box-text"
      >
        Aa
      </span>
    </div>
    <p>Contrast Ratio: {{ protanopiaContrastRatio }}:1</p>
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
          <td :class="protanopiaAANormal ? 'pass' : 'fail'">
            {{ (protanopiaAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="protanopiaAALarge ? 'pass' : 'fail'">
            {{ (protanopiaAALarge) ? '✓ PASS' : '✗ FAIL' }}
          </td>
        </tr>
        <tr>
          <th>AAA</th>
          <td :class="protanopiaAAANormal ? 'pass' : 'fail'">
            {{ (protanopiaAAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="protanopiaAAALarge ? 'pass' : 'fail'">
            {{ (protanopiaAAALarge) ? '✓ PASS' : '✗ FAIL' }}
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
  name: 'ProtanopiaCheck',
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
      protanopiaBackgroundColor: '',
      protanopiaTextColor: '',
      protanopiaContrastRatio: '',
      protanopiaAANormal: false,
      protanopiaAALarge: false,
      protanopiaAAANormal: false,
      protanopiaAAALarge: false
    }
  },
  watch: {
    backgroundColor () {
      this.checkProtanopiaColorContrast(this.textColor, this.backgroundColor)
    },
    textColor () {
      this.checkProtanopiaColorContrast(this.textColor, this.backgroundColor)
    }
  },
  mounted () {
    this.checkProtanopiaColorContrast(this.textColor, this.backgroundColor)
  },
  methods: {
    checkProtanopiaColorContrast (txt, bg) {
      this.protanopiaBackgroundColor = blinder.protanopia(bg)
      this.protanopiaTextColor = blinder.protanopia(txt)
      this.protanopiaContrastRatio = colord(this.protanopiaTextColor).contrast(this.protanopiaBackgroundColor)
      this.protanopiaAANormal = this.protanopiaContrastRatio >= 4.5
      this.protanopiaAALarge = this.protanopiaContrastRatio >= 3
      this.protanopiaAAANormal = this.protanopiaContrastRatio >= 7
      this.protanopiaAAALarge = this.protanopiaContrastRatio >= 4.5
    }
  }
}
</script>

<style lang="scss">
.protanopia {
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
