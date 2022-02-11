<template>
  <div id="app">
    <div v-html="doIt" />
    <ColorCard :colors="colors" :edit-colors="editColors" />
    <button @click="editColors = !editColors">
      Edit Color Palette
    </button>
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
              <Deuteranomaly :text-color="color" :background-color="color" :show-impairments="showImpairments" />
              <Deuteranopia :text-color="color" :background-color="color" :show-impairments="showImpairments" />
              <Tritanomaly :text-color="color" :background-color="color" :show-impairments="showImpairments" />
              <Tritanopia :text-color="color" :background-color="color" :show-impairments="showImpairments" />
              <Achromatomaly :text-color="color" :background-color="color" :show-impairments="showImpairments" />
              <Achromatopsia :text-color="color" :background-color="color" :show-impairments="showImpairments" />
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
          hex: '#FFFFFF'
        },
        {
          id: 2,
          name: 'Red',
          hex: '#FF0000'
        },
        {
          id: 3,
          name: 'Orange',
          hex: '#FFA500'
        },
        {
          id: 4,
          name: 'Yellow',
          hex: '#FFFF00'
        },
        {
          id: 5,
          name: 'Green',
          hex: '#00FF00'
        },
        {
          id: 6,
          name: 'Blue',
          hex: '#0000FF'
        },
        {
          id: 7,
          name: 'Black',
          hex: '#000000'
        }
      ],
      editColors: false,
      showImpairments: false,
      doIt: ''
    }
  },
  mounted () {
    this.doIt = this.dothis(this.colors)
  },
  methods: {
    dothis: (arr) => {
      let result = ''
      for (let counter1 = 0; counter1 < arr.length; counter1++) {
        for (let counter2 = 0; counter2 < arr.length; counter2++) {
          if (counter1 === 0 && counter2 === 0) {
            result += '[' + arr[counter1].hex + ']: <br />'
            result += '[' + arr[counter1].hex + ' X ' + arr[counter2].hex + '] ' + '<br />'
          } else if (counter1 === 0 && counter2 > 0) {
            result += '[' + arr[counter1].hex + ' X ' + arr[counter2].hex + '] ' + '<br />'
          } else if (counter2 === 0 && counter1 > 0) {
            result += '[' + arr[counter1].hex + ']: <br />'
            result += '[' + arr[counter1].hex + ' X ' + arr[counter2].hex + '] ' + '<br />'
          } else if (counter1 > 0 && counter2 > 0) {
            result += '[' + arr[counter1].hex + ' X ' + arr[counter2].hex + '] ' + '<br />'
          }
        }
      }

      return result
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
