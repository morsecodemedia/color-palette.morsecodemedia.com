<template>
  <div id="app">
    <ColorCard :colors="colors" />
    <div class="accessiblity-table-container">
      <table width="100%" border="1">
        <thead>
          <tr>
            <th>&nbsp;</th>
            <td
              v-for="(c, i) in colors"
              :key="i"
            >
              {{ c.name }} Text
            </td>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(c, i) in colors" :key="i">
            <td valign="top">
              {{ c.name }} background
              <button @click="showImpairments = !showImpairments">
                <span v-if="showImpairments">Hide</span>
                <span v-else>Show</span>
                Vision Impairment Simulations
              </button>
            </td>
            <td v-for="(color, index) in colors" :key="index">
              <Trichromatic :text-color="color" :background-color="color" />
              <Protanomaly :text-color="color" :background-color="color" :show-impairments="showImpairments" />
              <Protanopia :text-color="color" :background-color="color" :show-impairments="showImpairments" />
              <Deuteranomaly :color="color" :show-impairments="showImpairments" />
              <Deuteranopia :color="color" :show-impairments="showImpairments" />
              <Tritanomaly :color="color" :show-impairments="showImpairments" />
              <Tritanopia :color="color" :show-impairments="showImpairments" />
              <Achromatomaly :color="color" :show-impairments="showImpairments" />
              <Achromatopsia :color="color" :show-impairments="showImpairments" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import ColorCard from '~/components/global/color-card.vue'
import Achromatomaly from '~/components/global/achromatomaly.vue'
import Achromatopsia from '~/components/global/achromatopsia.vue'
import Deuteranomaly from '~/components/global/deuteranomaly.vue'
import Deuteranopia from '~/components/global/deuteranopia.vue'
import Protanomaly from '~/components/global/protanomaly.vue'
import Protanopia from '~/components/global/protanopia.vue'
import Trichromatic from '~/components/global/trichromatic.vue'
import Tritanomaly from '~/components/global/tritanomaly.vue'
import Tritanopia from '~/components/global/tritanopia.vue'
export default {
  name: 'HomePage',
  components: {
    ColorCard,
    Trichromatic,
    Protanomaly,
    Protanopia,
    Deuteranomaly,
    Deuteranopia,
    Tritanomaly,
    Tritanopia,
    Achromatomaly,
    Achromatopsia
  },
  data () {
    return {
      colors: [
        {
          id: 1,
          name: 'White',
          hex: '#FFFFFF',
          rgb: 'rbg(255, 255, 255)',
          hsl: 'hsl(0, 0, 100)'
        },
        {
          id: 2,
          name: 'Red',
          hex: '#FF0000',
          rgb: 'rgb(255, 0, 0)',
          hsl: 'hsl(0, 100, 50)'
        },
        {
          id: 3,
          name: 'Orange',
          hex: '#FFA500',
          rgb: 'rgb(255, 165, 0)',
          hsl: 'hsl(39, 100, 50)'
        },
        {
          id: 4,
          name: 'Yellow',
          hex: '#FFFF00',
          rgb: 'rgb(255, 255, 0)',
          hsl: 'hsl(60, 100, 50)'
        },
        {
          id: 5,
          name: 'Green',
          hex: '#00FF00',
          rgb: 'rgb(0, 255, 00)',
          hsl: 'hsl(120, 100, 50)'
        },
        {
          id: 6,
          name: 'Blue',
          hex: '#0000FF',
          rgb: 'rgb(0, 0, 255)',
          hsl: 'hsl(240, 100, 50)'
        },
        {
          id: 7,
          name: 'Black',
          hex: '#000000',
          rgb: 'rgb(0, 0, 0)',
          hsl: 'hsl(0, 0, 0)'
        }
      ],
      showImpairments: false
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
