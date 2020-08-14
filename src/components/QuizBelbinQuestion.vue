<template>
    <div>
        <md-content class="md-elevation-1">
            <md-badge class="md-primary" :md-content="remaining">
                <p>{{ title }}</p>
            </md-badge>
            <table>
                <tr v-for="(item,index) in items"
                    :key="index" :index="index" >
                    <td>{{ item }}</td>
                    <td>
                        <input 
                        type="range" max="10"
                        v-model.number="amounts[index]"
                        @change="sliderChanged(index)" />
                    </td>
                    <td>{{ amounts[index] }}</td>
                </tr>
            </table>
        </md-content>
    </div>
</template>


<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
    name: 'QuizBelbinQuestion',
    props: {
        title: String,
        items: Array,
    },
    data: () => ({
        lastIndex: null,
        amounts: [ 0, 0, 0, 0, 0, 0, 0, 0 ],
        remaining: 0
    }),
    created: function() {
        this.remaining = this.remainingPoints;
    },
    computed: {
        remainingPoints: function() { return 10 - this.amounts.reduce((sum, current) => sum + current, 0) }
    },
    methods: {
        sliderChanged: function (index) {
            this.lastIndex = index;
            let totalAmount = this.amounts.reduce((sum, current) => sum + current, 0);
            let nbSet = this.amounts.reduce((sum, current) => sum + (current>0?1:0), 0);
            let nbLoops = 0;
            while (nbLoops < 5 && (totalAmount>10 || nbSet>3)) {
                let lastChangeableIndex = null;
                for (let i = 0; i < this.amounts.length; i++) {
                    if (index !== i && this.amounts[i] > 0) {
                        lastChangeableIndex = i;
                    }
                }
                if (nbSet > 3) {
                    this.amounts[lastChangeableIndex] = 0;
                } else {
                    if (totalAmount>10) {
                        this.amounts[lastChangeableIndex] = this.amounts[lastChangeableIndex] - (totalAmount - 10);
                        if (this.amounts[lastChangeableIndex] < 0) {
                            this.amounts[lastChangeableIndex] = 0;
                        }
                    }
                }
                totalAmount = this.amounts.reduce((sum, current) => sum + current, 0);
                nbSet = this.amounts.reduce((sum, current) => sum + (current>0?1:0), 0);
                ++nbLoops;
            }
            this.remaining = this.remainingPoints;
            this.$forceUpdate();
        }
    }
})
</script>

<style scoped>
</style>