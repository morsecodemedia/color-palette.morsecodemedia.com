<template>
  <div class="trichromatic show">
    <p class="title">
      Regular Vision (Trichromatic)
    </p>
    <p class="subtitle">
      Can distinguish all three primary colors, little to no blurriness
    </p>
    <div :style="{ backgroundColor: color.hex }" class="color-box-background">
      <span class="color-box-text" :style="{ color: color.hex }"> Aa </span>
    </div>
    <p>Contrast Ratio: #.##:1</p>
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
          <td>Pass/Fail</td>
          <td>Pass/Fail</td>
        </tr>
        <tr>
          <th>AAA</th>
          <td>Pass/Fail</td>
          <td>Pass/Fail</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'TrichromaticCheck',
  props: {
    color: {
      type: Object,
      required: true,
      twoWay: true,
      default: () => {}
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
}
</style>
