<template>
    <div>
        <div class="regions">
            <select id="region-list" v-model="activeRegion" v-on:change="minUnitPrice = Number.MAX_VALUE">
                <option disabled>
                  Choose a region
                </option>
                <option v-for="region in Object.keys(beerSizes)" v-bind:value="region" v-bind:key="region">
                  {{ region }}
                </option>
            </select>
        </div>
        <div v-if="typeof(beerSizes[activeRegion]) !=='undefined'">
            <table id="size-list">
                <tr>
                  <th>Size</th>
                  <th>Price</th>
                  <th>Price to beat</th>
                  <th>Price per 425mL</th>
                </tr>
                <tr v-for="size in beerSizes[activeRegion]" v-bind:key="size.name" v-bind:class="{ highlight: size.price/size.metric === minUnitPrice }">
                  <td v-if="size.name != 'Custom'">
                    {{ size.name }} ({{ size.metric }}mL)
                  </td>
                  <td v-else>
                    <input type="number" value="500" v-model.number="size.metric" class="custom-metric" /> mL
                  </td>
                  <td>
                    <div class="price">
                      <span class="currency">$</span>
                      <input type="number" v-model.number="size.price" v-on:keyup="getSetMinUnitPrice(activeRegion)" class="custom-price" />
                    </div>
                  </td>
                  <td>
                    {{ equivalentUnitPrice(activeRegion, size.metric) }}
                  </td>
                  <td>
                    {{ isNaN(size.price) || size.price.length == 0 ? "-" : priceFormat(size.price / size.metric * 425) }}
                  </td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import axios from 'axios';

@Component
export default class BeerUnitPrice extends Vue {
  private activeRegion = '';
  private minUnitPrice = Number.MAX_VALUE;

  private beerSizes: any = {
    'Victoria':
    [
      { name: 'Pot', metric: 285, isMin: false },
      { name: 'Schooner', metric: 425, isMin: false },
      { name: 'Pint', metric: 570, isMin: false },
      { name: 'Jug', metric: 1140, isMin: false },
      { name: 'Custom', metric: 500, isMin: false },
    ],
    'South Australia':
      [
      { name: 'Schooner', metric: 285, isMin: false },
      { name: 'Pint', metric: 425, isMin: false },
      { name: 'Imperial Pint', metric: 570, isMin: false },
      { name: 'Jug', metric: 1140, isMin: false },
      { name: 'Custom', metric: 500, isMin: false },
    ],
    'New South Wales':
      [
          { name: 'Middy/Half Pint', metric: 285, isMin: false },
          { name: 'Schooner', metric: 425, isMin: false },
          { name: 'Pint', metric: 570, isMin: false },
          { name: 'Jug', metric: 1140, isMin: false },
          { name: 'Custom', metric: 500, isMin: false },
      ],
    'Queensland':
    [
        { name: 'Pot', metric: 285, isMin: false },
        { name: 'Schooner', metric: 425, isMin: false },
        { name: 'Pint', metric: 570, isMin: false },
        { name: 'Jug', metric: 1140, isMin: false },
        { name: 'Custom', metric: 500, isMin: false },
    ],
    'Tasmania':
    [
        { name: 'Pot', metric: 285, isMin: false },
        { name: 'Schooner/Fifteen', metric: 425, isMin: false },
        { name: 'Pint', metric: 570, isMin: false },
        { name: 'Jug', metric: 1140, isMin: false },
        { name: 'Custom', metric: 500, isMin: false },
    ],
    'Northern Territory':
    [
        { name: 'Handle', metric: 285, isMin: false },
        { name: 'Schooner', metric: 425, isMin: false },
        { name: 'Pint', metric: 570, isMin: false },
        { name: 'Jug', metric: 1140, isMin: false },
        { name: 'Custom', metric: 500, isMin: false },
    ],
    'Western Australia':
    [
        { name: 'Middy/Half Pint', metric: 285, isMin: false },
        { name: 'Schooner', metric: 425, isMin: false },
        { name: 'Pint', metric: 570, isMin: false },
        { name: 'Jug', metric: 1140, isMin: false },
        { name: 'Custom', metric: 500, isMin: false },
    ],
    'New Zealand':
    [
        { name: 'Half-Pint', metric: 250, isMin: false },
        { name: 'Handle', metric: 425, isMin: false },
        { name: 'Pint', metric: 500, isMin: false },
        { name: 'Jug', metric: 900, isMin: false },
        { name: 'Custom', metric: 500, isMin: false },
    ],
  };

  public priceFormat(n: number) {
    // Round to 2 decimal points
    const rounded = Math.round(n * 100).toString();

    // Insert decimal point 2 positions from the end
    return '$' + rounded.slice(0, rounded.length - 2) + '.' + rounded.slice(rounded.length - 2);
  }

  private getRegion() {
    axios.get('https://ipapi.co/json/')
    .then((response) => {
      this.activeRegion = response.data.region;
    });
  }

  private getSetMinUnitPrice(region: string) {
    this.minUnitPrice = Number.MAX_VALUE;

    for (const size of this.beerSizes[region]) {
      const unitPrice = size.price / size.metric;
      if (isNaN(unitPrice) || size.price.length === 0) {
        continue;
      }
      if (unitPrice < this.minUnitPrice) {
        this.minUnitPrice = unitPrice;
      }
    }
  }

  private equivalentUnitPrice(region: string, volume: number) {
    return this.minUnitPrice === Number.MAX_VALUE ? '-' : this.priceFormat(this.minUnitPrice * volume);
  }

  private created() {
    this.getRegion();
  }
}
</script>

<style>
ul {
  list-style: none;
  padding: 0;
}
li {
  display: inline;
  margin-right: 10px;
}
li > button {
  display: inline-block;
  padding: 9px;
  color: #c63c22;
  background-color: #ffece9;
  border: 1px solid #ffece9;
  margin-bottom: 10px;
  text-decoration: none;
  font-size: 100%;
}
li.active > button {
  border-color: #c77667;
  font-weight: bold;
}
select {
  padding: 10px;
  margin-bottom: 20px;
}
table {
  margin: 0 auto;
}
.highlight {
  background: #ffece9;
}
.price {
  position: relative;
}
.currency {
  position: absolute;
  left: 10px;
  top: 12px;
  color: #a9a9a9;
  font-size: 120%;
}
input {
  padding: 10px;
  font-size: 120%;
}
.currency + input {
  padding-left: 23px;
}
input.custom-price {
  width: 50px;
}
input.custom-metric {
  width: 40px;
}
input[type='number'] {
  -moz-appearance:textfield;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}
</style>