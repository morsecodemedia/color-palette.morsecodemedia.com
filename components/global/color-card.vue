<template>
  <div class="color-cards-container">
    <div v-for="(c, i) in colors" :key="i" class="color-card">
      <div
        v-if="!editColors"
        :style="{ backgroundColor: c.hex }"
        class="color-block"
      />
      <div
        v-else
        class="color-block-editor"
      >
        <input
          v-model="c.hex"
          type="color"
          class="color-picker"
        >
        <input
          v-model="c.name"
          placeholder="Name of Color"
          type="text"
          value="c.name"
        >
        <input
          v-model="c.hex"
          type="text"
        >
      </div>
      <div
        v-if="!editColors"
        class="color-details"
      >
        <span class="color-name">{{ (c.name !== '') ? c.name : 'Color ' + i }}</span>
        <span class="color-hex">Hex: {{ c.hex }}</span>
        <span class="color-rgb">RGB: {{ hexToRGB(c.hex) }}</span>
        <span class="color-hsl">HSL: {{ hexToHSL(c.hex) }}</span>
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

    .color-block-editor {
      width: 200px;
      margin-bottom: 10px;
      .color-picker {
        height: 200px;
        width: 200px;
        margin-bottom: 10px;
      }
      input {
        margin-bottom: 10px;
      }
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
