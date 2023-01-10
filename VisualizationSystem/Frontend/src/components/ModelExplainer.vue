<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-01-10 23:08:34
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Model Explainer</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="modelExplainer" style="height: 100%; width: calc(50% - 7.5px); float: right;">
            <svg id="modelExplainer" height="100%" width="100%">
                
                <g>
                    <text font-weight="bold" x="15" y="15">Correlation</text>
                    <text font-weight="bold" :x="elWidth - 60" :y="elHeight - 35">RMSE</text>
                    <g v-for="(item, i) in xAxis">
                        <path :d="'M ' + item.x + ' ' + item.y + ' L ' + + item.x + ' ' + (item.y - 5)" stroke="black">
                        </path>
                        <path :d="'M ' + item.x + ' ' + item.y + ' L ' + + item.x + ' ' + (20)"
                            stroke="rgba(0, 0, 0, 0.5)" stroke-dasharray="5"></path>
                        <text :x="item.x" :y="elHeight - 10" dx="-0.3em">{{ item.t }}</text>
                    </g>
                    <g v-for="(item, i) in yAxis">
                        <path :d="'M ' + (item.x + 25) + ' ' + item.y + ' L ' + + (item.x + 30) + ' ' + (item.y)"
                            stroke="black"></path>
                        <path :d="'M ' + (elWidth - 20) + ' ' + item.y + ' L ' + + (item.x + 30) + ' ' + (item.y)"
                            stroke="rgba(0, 0, 0, 0.5)" stroke-dasharray="5"></path>
                        <text :x="0" :y="item.y" dy="0.3em">{{ item.t }}</text>
                    </g>
                </g>
                <g>
                    <circle v-for="(item, i) in nodeData" :cx="item.x" :cy="item.y" :r="3" fill="steelblue"></circle>
                </g>
            </svg>
        </div>
    </div>
</template>
<script>
import { scaleLinear } from 'd3-scale';
export default {
    name: 'modelExplainerView',
    props: [],
    data() {
        return {
            elHeight: 0,
            elWidth: 0,
            nodeData: [],
            xAxis: [],
            yAxis: []
        }
    },
    methods: {
        translate(x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        },
        calcAxis() {
            const xScale = scaleLinear([0, 1], [30, this.elWidth - 20]);
            const yScale = scaleLinear([0, 1], [this.elHeight - 30, 20]);
            let xAxis = [];
            let yAxis = [];
            for (let i = 0; i <= 10; ++i) {
                xAxis.push({
                    t: i / 10,
                    x: xScale(i / 10),
                    y: this.elHeight - 25,
                    // d: 
                });
                yAxis.push({
                    t: i / 10,
                    y: yScale(i / 10),
                    x: 0
                });
            }
            return [xAxis, yAxis];
        },
        calcNode(data) {
            let nodeData = [];
            const xScale = scaleLinear([0, 1], [30, this.elWidth - 20]);
            const yScale = scaleLinear([0, 1], [this.elHeight - 30, 20]);
            for (let i = 0; i < 76; ++i) {
                let rx = Math.random();
                let ry = Math.random();
                nodeData.push({
                    x: xScale(rx),
                    y: yScale(ry),
                    r: [rx, ry]
                });
            }
            return nodeData;
        }
    },
    created() {
    },
    mounted() {
        this.elHeight = this.$refs.modelExplainer.offsetHeight;
        this.elWidth = this.$refs.modelExplainer.offsetWidth;
        this.nodeData = this.calcNode(1);
        console.log(this.nodeData);
        [this.xAxis, this.yAxis] = this.calcAxis();
        console.log(this.xAxis, this.yAxis)
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
