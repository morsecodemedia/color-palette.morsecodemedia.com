<template>
  <div
    class="tritanopia"
    :class="{ show: showImpairments }"
  >
    <p class="title">
      Tritanopia
    </p>
    <p class="subtitle">
      Blue blind - Can't see blues at all
    </p>
    <div
      :style="{ backgroundColor: tritanopiaBackgroundColor }"
      class="color-box-background"
    >
      <span
        :style="{ color: tritanopiaTextColor }"
        class="color-box-text"
      >
        Aa
      </span>
    </div>
    <p>Contrast Ratio: {{ tritanopiaContrastRatio }}:1</p>
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
          <td :class="tritanopiaAANormal ? 'pass' : 'fail'">
            {{ (tritanopiaAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="tritanopiaAALarge ? 'pass' : 'fail'">
            {{ (tritanopiaAALarge) ? '✓ PASS' : '✗ FAIL' }}
          </td>
        </tr>
        <tr>
          <th>AAA</th>
          <td :class="tritanopiaAAANormal ? 'pass' : 'fail'">
            {{ (tritanopiaAAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="tritanopiaAAALarge ? 'pass' : 'fail'">
            {{ (tritanopiaAAALarge) ? '✓ PASS' : '✗ FAIL' }}
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
  name: 'TritanopiaCheck',
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
      tritanopiaBackgroundColor: '',
      tritanopiaTextColor: '',
      tritanopiaContrastRatio: '',
      tritanopiaAANormal: false,
      tritanopiaAALarge: false,
      tritanopiaAAANormal: false,
      tritanopiaAAALarge: false
    }
  },
  watch: {
    backgroundColor () {
      this.checkTritanopiaColorContrast(this.textColor.hex, this.backgroundColor.hex)
    },
    textColor () {
      this.checkTritanopiaColorContrast(this.textColor.hex, this.backgroundColor.hex)
    }
  },
  mounted () {
    this.checkTritanopiaColorContrast(this.textColor.hex, this.backgroundColor.hex)
  },
  methods: {
    checkTritanopiaColorContrast (txt, bg) {
      this.tritanopiaBackgroundColor = blinder.tritanopia(bg.slice(0, -2))
      this.tritanopiaTextColor = blinder.tritanopia(txt.slice(0, -2))
      this.tritanopiaContrastRatio = colord(this.tritanopiaTextColor).contrast(this.tritanopiaBackgroundColor)
      this.tritanopiaAANormal = this.tritanopiaContrastRatio >= 4.5
      this.tritanopiaAALarge = this.tritanopiaContrastRatio >= 3
      this.tritanopiaAAANormal = this.tritanopiaContrastRatio >= 7
      this.tritanopiaAAALarge = this.tritanopiaContrastRatio >= 4.5
    }
  }
}
</script>

<style lang="scss">
.tritanopia {
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
