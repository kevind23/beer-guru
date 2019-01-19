<template>
    <div class="regions">
        <h2>Choose a Region</h2>
        <ul id="region-list">
            <li v-for="region in regions" v-bind:class="[region.name == activeRegion ? 'active' : '']">
                <a v-on:click.prevent="$emit('regionChange', region.name); activeRegion = region.name;" href="#">
                    {{ region.name }}
                </a>
            </li>
        </ul>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component({
    props: [ 'activeRegion' ],
})
export default class RegionList extends Vue {
    private regions = [
        { name: 'Victoria' },
        { name: 'South Australia' },
        { name: 'Western Australia' },
        { name: 'Northern Territory' },
        { name: 'Queensland' },
        { name: 'New South Wales' },
        { name: 'Tasmania' },
        { name: 'New Zealand' },
    ];
}
</script>