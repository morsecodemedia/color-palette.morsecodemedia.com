<template>
  <div class="color-cards-container">
    <div v-for="(c, i) in colors" :key="i" class="color-card">
      <div :style="{ backgroundColor: c.hex }" class="color-block" />
      <div class="color-details">
        <span class="color-name">{{ c.name }}</span>
        <span class="color-hex">Hex: {{ c.hex }}</span>
        <span class="color-rgb">RGB: {{ c.rgb }}</span>
        <span class="color-hsl">HSL: {{ c.hsl }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ColorCard',
  props: {
    colors: {
      type: Array,
      required: true,
      twoWay: true,
      default: () => {}
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
