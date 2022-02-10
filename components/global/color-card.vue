<template>
  <div class="color-cards-container">
    <div v-for="(c, i) in colors" :key="i" class="color-card">
      <div v-if="!editColors" :style="{ backgroundColor: c.hex }" class="color-block" />
      <!-- Color Picker Here -->
      <div class="color-details">
        <span v-if="!editColors" class="color-name">{{ (c.name !== '') ? c.name : 'Color ' + i }}</span>
        <input v-else v-model="c.name" placeholder="Name of Color" type="text" value="c.name">
        <span v-show="!editColors" class="color-hex">Hex: {{ c.hex }}</span>
        <span v-show="!editColors" class="color-rgb">RGB: {{ hexToRGB(c.hex) }}</span>
        <span v-show="!editColors" class="color-hsl">HSL: {{ hexToHSL(c.hex) }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import { colord } from 'colord'

export default {
  name: 'ColorCard',
  props: {
    colors: {
      type: Array,
      required: true,
      twoWay: true,
      default: () => {}
    },
    editColors: {
      type: Boolean,
      required: true,
      default: () => {}
    }
  },
  methods: {
    hexToRGB (hex) {
      return colord(hex).toRgbString()
    },
    hexToHSL (hex) {
      return colord(hex).toHslString()
    }
  }
}
</script>

<style lang="scss">
.color-cards-container {
  display: flex;
  justify-content: space-evenly;
  .color-card {
    padding: 10px;
    border-radius: 15px;
    border: 1px solid black;
    display: inline-flex;
    flex-direction: column;
    justify-content: center;
    align-content: center;

    .color-block {
      height: 200px;
      width: 200px;
      border: 1px solid black;
      margin-bottom: 10px;
    }

    .color-block-picker {
      margin-bottom: 10px;
    }

    .color-details {
      display: flex;
      flex-direction: column;
      text-align: left;

      span {
        margin-bottom: 5px;
      }
    }
  }
}
</style>
