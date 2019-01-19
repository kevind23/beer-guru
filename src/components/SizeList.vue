<template>
    <div v-if="typeof(beerSizes[activeRegion]) !=='undefined'">
        <h2>Choose a Size</h2>
        <ul id="size-list">
            <li v-for="size in beerSizes[activeRegion]">
                {{ size.name }} ({{ size.metric }}mL)
                <br />
                <input value="$" v-model="size.price" v-on:input="size.price = (size.price.charAt(0) == '$' ? '' : '$') + size.price" />
                <p v-if="!isNaN(size.price.slice(1)) && size.price.length > 1">
                Price per 425mL
                <br />
                ${{ Math.round(parseFloat(size.price.slice(1)) / size.metric * 42500) / 100 }}
                </p>
            </li>
        </ul>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component({
    props: ['activeRegion'],
})
export default class SizeList extends Vue {
  private beerSizes = {
    'Victoria':
    [
      { name: 'Pony', metric: 140, price: '$' },
      { name: 'Pot', metric: 285, price: '$' },
      { name: 'Schooner', metric: 425, price: '$' },
      { name: 'Pint', metric: 570, price: '$' },
      { name: 'Jug', metric: 1140, price: '$' },
    ],
    'South Australia':
      [
      { name: 'Pony', metric: 140, price: '$' },
      { name: 'Butcher', metric: 200, price: '$' },
      { name: 'Schooner', metric: 285, price: '$' },
      { name: 'Pint', metric: 425, price: '$' },
      { name: 'Imperial Pint', metric: 570, price: '$' },
      { name: 'Jug', metric: 1140, price: '$' },
    ],
    'New South Wales':
      [
          { name: 'Pony', metric: 140, price: '$' },
          { name: 'Seven', metric: 200, price: '$' },
          { name: 'Middy/Half Pint', metric: 285, price: '$' },
          { name: 'Schmiddy', metric: 350, price: '$' },
          { name: 'Schooner', metric: 425, price: '$' },
          { name: 'Pint', metric: 570, price: '$' },
          { name: 'Jug', metric: 1140, price: '$' },
      ],
    'Queensland':
    [
        { name: 'Pony', metric: 140, price: '$' },
        { name: 'Seven', metric: 200, price: '$' },
        { name: 'Pot', metric: 285, price: '$' },
        { name: 'Schooner', metric: 425, price: '$' },
        { name: 'Pint', metric: 570, price: '$' },
        { name: 'Jug', metric: 1140, price: '$' },
    ],
    'Tasmania':
    [
        { name: 'Small beer', metric: 115, price: '$' },
        { name: 'Six', metric: 170, price: '$' },
        { name: 'Seven', metric: 200, price: '$' },
        { name: 'Pot', metric: 285, price: '$' },
        { name: 'Schooner/Fifteen', metric: 425, price: '$' },
        { name: 'Pint', metric: 570, price: '$' },
        { name: 'Jug', metric: 1140, price: '$' },
    ],
    'Northern Territory':
    [
        { name: 'Seven', metric: 200, price: '$' },
        { name: 'Handle', metric: 285, price: '$' },
        { name: 'Schooner', metric: 425, price: '$' },
        { name: 'Pint', metric: 570, price: '$' },
        { name: 'Jug', metric: 1140, price: '$' },
    ],
    'Western Australia':
    [
        { name: 'Shetland', metric: 115, price: '$' },
        { name: 'Pony', metric: 140, price: '$' },
        { name: 'Bobbie/Six', metric: 170, price: '$' },
        { name: 'Glass', metric: 200, price: '$' },
        { name: 'Middy/Half Pint', metric: 285, price: '$' },
        { name: 'Schooner', metric: 425, price: '$' },
        { name: 'Pint', metric: 570, price: '$' },
        { name: 'Jug', metric: 1140, price: '$' },
    ],
    'New Zealand':
    [
        { name: 'Half-Pint', metric: 250, price: '$' },
        { name: 'Handle', metric: 425, price: '$' },
        { name: 'Pint', metric: 500, price: '$' },
        { name: 'Jug', metric: 900, price: '$' },
    ],
  };
}
</script>