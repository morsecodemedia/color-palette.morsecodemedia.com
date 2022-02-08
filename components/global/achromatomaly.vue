<template>
  <div
    class="achromatomaly"
    :class="{ show: showImpairments }"
  >
    <p class="title">
      Achromatomaly
    </p>
    <p class="subtitle">
      Partial color blindness, sees the absence of most colors
    </p>
    <div
      :style="{ backgroundColor: achromatomalyBackgroundColor }"
      class="color-box-background"
    >
      <span
        :style="{ color: achromatomalyTextColor }"
        class="color-box-text"
      >
        Aa
      </span>
    </div>
    <p>Contrast Ratio: {{ achromatomalyContrastRatio }}:1</p>
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
          <td :class="achromatomalyAANormal ? 'pass' : 'fail'">
            {{ (achromatomalyAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="achromatomalyAALarge ? 'pass' : 'fail'">
            {{ (achromatomalyAALarge) ? '✓ PASS' : '✗ FAIL' }}
          </td>
        </tr>
        <tr>
          <th>AAA</th>
          <td :class="achromatomalyAAANormal ? 'pass' : 'fail'">
            {{ (achromatomalyAAANormal) ? '✓ PASS' : '✗ FAIL' }}
          </td>
          <td :class="achromatomalyAAALarge ? 'pass' : 'fail'">
            {{ (achromatomalyAAALarge) ? '✓ PASS' : '✗ FAIL' }}
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
  name: 'AchromatomalyCheck',
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
      achromatomalyBackgroundColor: '',
      achromatomalyTextColor: '',
      achromatomalyContrastRatio: '',
      achromatomalyAANormal: false,
      achromatomalyAALarge: false,
      achromatomalyAAANormal: false,
      achromatomalyAAALarge: false
    }
  },
  watch: {
    backgroundColor () {
      this.checkAchromatomalyColorContrast(this.textColor.hex, this.backgroundColor.hex)
    },
    textColor () {
      this.checkAchromatomalyColorContrast(this.textColor.hex, this.backgroundColor.hex)
    }
  },
  mounted () {
    this.checkAchromatomalyColorContrast(this.textColor.hex, this.backgroundColor.hex)
  },
  methods: {
    checkAchromatomalyColorContrast (txt, bg) {
      this.achromatomalyBackgroundColor = blinder.achromatomaly(bg.slice(0, -2))
      this.achromatomalyTextColor = blinder.achromatomaly(txt.slice(0, -2))
      this.achromatomalyContrastRatio = colord(this.achromatomalyTextColor).contrast(this.achromatomalyBackgroundColor)
      this.achromatomalyAANormal = this.achromatomalyContrastRatio >= 4.5
      this.achromatomalyAALarge = this.achromatomalyContrastRatio >= 3
      this.achromatomalyAAANormal = this.achromatomalyContrastRatio >= 7
      this.achromatomalyAAALarge = this.achromatomalyContrastRatio >= 4.5
    }
  }
}
</script>

<style lang="scss">
.achromatomaly {
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
