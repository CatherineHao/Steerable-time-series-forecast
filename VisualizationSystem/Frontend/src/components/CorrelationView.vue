<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-01-12 02:11:55
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Correlation View</div>
        <p class="titleTriangle"></p>
        <svg height="30px" width="200px" style="float: right;">
                <rect v-for="(t, i) in legendRect" :key="'lr' + i" :x="20 + i * (150 / legendRect.length)" y="10" height="15" :width="(150 / legendRect.length)" :fill="t" :stroke="t"></rect>
                <text x="0" y="23">-1</text>
                <text x="175" y="23">1</text>
            </svg>
    </div>
    <div class="frameworkBody">
        <div ref="CorrelationView" style="height: 100%; width: 100%;">
            <svg id="CorrelationView" height="100%" width="100%">
                <text v-for="(item, i) in smooth" x="10" :y="i * (elHeight) / 3 + 50" font-size="20">{{ item }}</text>
                <text v-for="(item, i) in sample" :x="110 + i * (elWidth - 80) / 3 + (i == 1 ? 38 : i == 2 ? 25 : 0)" :y="15" font-size="20">{{ item }}</text>
                <g v-for="(item, ii) in mData">
                    <rect v-for="(item1, i) in item[0]" :transform="translate(0, ii * (elHeight) / 3, 0)" :x="75 + (i % 8) * 18" :y="25 + 18 * (parseInt(i / 8))" :width="18" :height="18" :fill="item1" stroke="rgba(229, 229, 229)"></rect>
                    
                    <rect v-for="(item2, i) in item[1]" :transform="translate(180, ii * (elHeight) / 3, 0)" :x="75 + (i) * 18" :y="34" :width="18" :height="18" :fill="item2" stroke="rgba(229, 229, 229)"></rect>

                    
                    <rect v-for="(item2, i) in item[2]" :transform="translate(342, ii * (elHeight) / 3, 0)" :x="75 + (i) * 18" :y="34" :width="18" :height="18" :fill="item2" stroke="rgba(229, 229, 229)"></rect>
                </g>
            </svg>
        </div>
    </div>
</template>
<script>
import { interpolateRdBu } from 'd3';
export default {
    name: 'CorrelationView',
    props: [],
    data() {
        return {
            elHeight: 0,
            elWidth: 0,
            sample: ['10-slice', '7-slice', '3-slice'],
            smooth: ['RS', 'NA', 'E/H'],
            legendRect: [],
            mData: []
        }
    },
    methods: {
        translate(x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        },
        legend() {
            let colors = interpolateRdBu;
            let legendL = [];
            for (let i = 0 ; i <= 100; ++i) {
                legendL.push(colors(i / 100));
            }
            return legendL;
        },
        calcMatrix() {
            let colors = interpolateRdBu;
            let mData = [];
            for (let i = 1; i <= 3; ++i) {
                let td1 = [], td2 = [], td3 = [];
                for (let j = 0; j < 16; ++j) {
                    td1.push(colors(Math.random()));
                }
                for (let j = 0; j < 7; ++j) {
                    td2.push(colors(Math.random()));
                }
                for (let j = 0; j < 3; ++j) {
                    td3.push(colors(Math.random()));
                }
                mData.push([td1, td2, td3]);
            }
            return mData;
        }
    },
    created() {
    },
    mounted() {
        this.elHeight = this.$refs.CorrelationView.offsetHeight;
        this.elWidth = this.$refs.CorrelationView.offsetWidth;
        this.legendRect = this.legend();
        this.mData = this.calcMatrix();
        console.log(this.mData)
        // console.log(this.elHeight)
    },
}
</script>
<style scoped>
*,
*::before,
*::after {
    font-weight: normal;
}
</style>
