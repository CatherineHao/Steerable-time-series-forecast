<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-02-02 15:18:51
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Data Transformation Overview</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="DataTransformation" style="height: calc(55%); width: 100%; ">
            <svg id="DataTransformation" height="100%" width="100%">
                <!-- Transformation Tree -->
                <g>
                    <g>
                        <rect :x="elWidth / 2 - 75" :y="0" :width="150" :height="75" fill="#D9D9D9"></rect>
                        <text :x="elWidth / 2" :y="30" text-anchor="middle" font-size="24">Time</text>
                        <text :x="elWidth / 2" :y="60" text-anchor="middle" font-size="24">Series</text>

                    </g>
                    <g>
                        <path
                            :d="'M' + (elWidth / 2) + ' ' + 75 + ' L ' + (elWidth / 2) + ' ' + (75 + (elHeight - 75 * 3) / 2)"
                            stroke="black"></path>
                        <path :d="d[0]" stroke="black" :fill="'none'"></path>
                        <path :d="d[1]" stroke="black" :fill="'none'"></path>
                        <g :transform="translate(0, 75 + (elHeight - 75 * 3) / 2, 0)">
                            <rect :x="elWidth / 2 - 75" :y="0" :width="150" :height="75" fill="#D9D9D9"></rect>
                            <text :x="elWidth / 2" :y="45" text-anchor="middle" font-size="24">7-slice</text>
                        </g>
                        <g :transform="translate(-elWidth / 3, 75 + (elHeight - 75 * 3) / 2, 0)">
                            <rect :x="elWidth / 2 - 75" :y="0" :width="150" :height="75" fill="#D9D9D9"></rect>
                            <text :x="elWidth / 2" :y="45" text-anchor="middle" font-size="24">10-slice</text>
                        </g>
                        <g :transform="translate(elWidth / 3, 75 + (elHeight - 75 * 3) / 2, 0)">
                            <rect :x="elWidth / 2 - 75" :y="0" :width="150" :height="75" fill="#D9D9D9"></rect>
                            <text :x="elWidth / 2" :y="45" text-anchor="middle" font-size="24">3-slice</text>
                        </g>
                    </g>

                    <g :transform="translate(0, 150 + (elHeight - 75 * 3), 0)">
                        <path
                            :d="'M' + (elWidth / 2 - elWidth / 3) + ' ' + (-(elHeight - 75 * 3) / 2) + ' L ' + (elWidth / 2 - elWidth / 3) + ' ' + (0)"
                            stroke="black"></path>
                        <path :d="d[2]" stroke="black" :fill="'none'"></path>
                        <path :d="d[3]" stroke="black" :fill="'none'"></path>
                        <g>
                            <rect :x="0" :y="0" :width="(elWidth - 40) / 9" :height="75" fill="#D9D9D9"></rect>
                            <text :x="(elWidth - 40) / 18" :y="30" text-anchor="middle" font-size="24">Raw</text>
                            <text :x="(elWidth - 40) / 18" :y="60" text-anchor="middle" font-size="20">Sequence</text>

                        </g>
                        <g :transform="translate((elWidth - 40) / 9 + 5, 0, 0)">
                            <rect :x="0" :y="0" :width="(elWidth - 40) / 9" :height="75" fill="#D9D9D9"></rect>
                            <text :x="(elWidth - 40) / 18" :y="30" text-anchor="middle" font-size="24">N-</text>
                            <text :x="(elWidth - 40) / 18" :y="60" text-anchor="middle" font-size="20">Average</text>

                        </g>
                        <g :transform="translate(2 * (elWidth - 40) / 9 + 10, 0, 0)">
                            <rect :x="0" :y="0" :width="(elWidth - 40) / 9" :height="75" fill="#D9D9D9"></rect>
                            <text :x="(elWidth - 40) / 18" :y="30" text-anchor="middle" font-size="24">EMA/</text>
                            <text :x="(elWidth - 40) / 18" :y="60" text-anchor="middle" font-size="20">Holt</text>

                        </g>
                    </g>

                    <g :transform="translate((elWidth - 40) / 3 + 15, 150 + (elHeight - 75 * 3), 0)">
                        <path
                            :d="'M' + (elWidth / 2 - elWidth / 3) + ' ' + (-(elHeight - 75 * 3) / 2) + ' L ' + (elWidth / 2 - elWidth / 3) + ' ' + (0)"
                            stroke="black"></path>
                        <path :d="d[2]" stroke="black" :fill="'none'"></path>
                        <path :d="d[3]" stroke="black" :fill="'none'"></path>
                        <g>
                            <rect :x="0" :y="0" :width="(elWidth - 40) / 9" :height="75" fill="#D9D9D9"></rect>
                            <text :x="(elWidth - 40) / 18" :y="30" text-anchor="middle" font-size="24">Raw</text>
                            <text :x="(elWidth - 40) / 18" :y="60" text-anchor="middle" font-size="20">Sequence</text>

                        </g>
                        <g :transform="translate((elWidth - 40) / 9 + 5, 0, 0)">
                            <rect :x="0" :y="0" :width="(elWidth - 40) / 9" :height="75" fill="#D9D9D9"></rect>
                            <text :x="(elWidth - 40) / 18" :y="30" text-anchor="middle" font-size="24">N-</text>
                            <text :x="(elWidth - 40) / 18" :y="60" text-anchor="middle" font-size="20">Average</text>

                        </g>
                        <g :transform="translate(2 * (elWidth - 40) / 9 + 10, 0, 0)">
                            <rect :x="0" :y="0" :width="(elWidth - 40) / 9" :height="75" fill="#D9D9D9"></rect>
                            <text :x="(elWidth - 40) / 18" :y="30" text-anchor="middle" font-size="24">EMA/</text>
                            <text :x="(elWidth - 40) / 18" :y="60" text-anchor="middle" font-size="20">Holt</text>

                        </g>
                    </g>

                    <g :transform="translate(2 * (elWidth - 40) / 3 + 30, 150 + (elHeight - 75 * 3), 0)">
                        <path
                            :d="'M' + (elWidth / 2 - elWidth / 3) + ' ' + (-(elHeight - 75 * 3) / 2) + ' L ' + (elWidth / 2 - elWidth / 3) + ' ' + (0)"
                            stroke="black"></path>
                        <path :d="d[2]" stroke="black" :fill="'none'"></path>
                        <path :d="d[3]" stroke="black" :fill="'none'"></path>
                        <g>
                            <rect :x="0" :y="0" :width="(elWidth - 40) / 9" :height="75" fill="#D9D9D9"></rect>
                            <text :x="(elWidth - 40) / 18" :y="30" text-anchor="middle" font-size="24">Raw</text>
                            <text :x="(elWidth - 40) / 18" :y="60" text-anchor="middle" font-size="20">Sequence</text>

                        </g>
                        <g :transform="translate((elWidth - 40) / 9 + 5, 0, 0)">
                            <rect :x="0" :y="0" :width="(elWidth - 40) / 9" :height="75" fill="#D9D9D9"></rect>
                            <text :x="(elWidth - 40) / 18" :y="30" text-anchor="middle" font-size="24">N-</text>
                            <text :x="(elWidth - 40) / 18" :y="60" text-anchor="middle" font-size="20">Average</text>

                        </g>
                        <g :transform="translate(2 * (elWidth - 40) / 9 + 10, 0, 0)">
                            <rect :x="0" :y="0" :width="(elWidth - 40) / 9" :height="75" fill="#D9D9D9"></rect>
                            <text :x="(elWidth - 40) / 18" :y="30" text-anchor="middle" font-size="24">EMA/</text>
                            <text :x="(elWidth - 40) / 18" :y="60" text-anchor="middle" font-size="20">Holt</text>

                        </g>
                    </g>
                </g>
                <g v-for="(item, i) in barData" :key="'bar_g' + i"
                    :transform="translate(0, i * elHeight / barData.length, 0)">
                    <text x="0" y="1em">{{ item.slice }}</text>
                    <text x="4em" y="1em">{{ item.smooth }}</text>
                    <g v-for="(d, j) in item.bar_data" :key="'single_bar' + j" :transform="translate(0, 20, 0)">
                        <rect :x="d.train.x" :y="5" :height="20" :width="d.train.w" :fill="d.train.fill" :opacity="0.1">
                        </rect>
                        <path
                            :d="'M ' + d.test.x1 + ' 5 L ' + d.test.x1 + ' 25 L ' + d.test.x2 + ' 20 L ' + d.test.x2 + ' 10 Z'"
                            :fill="d.test.fill" :stroke="'black'"></path>
                    </g>
                </g>
            </svg>
        </div>
        <div ref="timeline" style="height: calc(45% - 15px); width: 100%; margin-top: 15px;">
            <svg id="timeline" height="100%" width="100%">

            </svg>
        </div>
    </div>
</template>
<script>
import { curveBumpY, line } from 'd3-shape';
import { scaleUtc, scaleLinear } from 'd3-scale';
import { axisLeft, axisBottom } from 'd3-axis';
import { parse } from '@babel/parser';
import { interpolateRdBu } from 'd3-scale-chromatic';
import { useDataStore } from "../stores/counter";
export default {
    name: 'DataTransformationView',
    props: ['timeData', 'sliceData'],
    data() {
        return {
            elHeight: 0,
            elWidth: 0,
            tlHeight: 0,
            tlWidth: 0,
            sample: ['10-slice', '7-slice', '3-slice'],
            smooth: ['Raw Sequence', 'N-Average', 'EMA/Holt'],
            d: [],
            startTime: 0,
            timeScale: null,
            maeScale: null,
            tiemSliceData: [],
            barData: []
        }
    },
    methods: {
        translate(x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        },
        calcCurve() {
            let p1 = [[this.elWidth / 2, 75], [this.elWidth / 2 - this.elWidth / 3, 75 + (this.elHeight - 75 * 3) / 2]];
            let p2 = [[this.elWidth / 2, 75], [this.elWidth / 2 + this.elWidth / 3, 75 + (this.elHeight - 75 * 3) / 2]];
            let p3 = [[this.elWidth / 2 - this.elWidth / 3, -((this.elHeight - 75 * 3) / 2)], [(this.elWidth - 40) / 18, 0]];
            let p4 = [[this.elWidth / 2 - this.elWidth / 3, -(this.elHeight - 75 * 3) / 2], [2 * (this.elWidth - 40) / 9 + (this.elWidth - 40) / 18 + 10, 0]];
            let cline = line().curve(curveBumpY);
            let d = [cline(p1), cline(p2), cline(p3), cline(p4)];
            return d;
        },
        clacTime(data) {
            let margin = ({ top: 20, right: 20, bottom: 30, left: 40 });
            let height = 440;
            let focusHeight = 100;
            let yAxis = (g, y, title) => g
                .attr("transform", `translate(${margin.left},0)`)
                .call(d3.axisLeft(y))
                .call(g => g.select(".domain").remove())
                .call(g => g.selectAll(".title").data([title]).join("text")
                    .attr("class", "title")
                    .attr("x", -margin.left)
                    .attr("y", 10)
                    .attr("fill", "currentColor")
                    .attr("text-anchor", "start")
                    .text(title));
            let xAxis = (g, x, height) => g
                .attr("transform", `translate(0,${height - margin.bottom})`)
                .call(d3.axisBottom(x).ticks(width / 80).tickSizeOuter(0));
            let y = d3.scaleLinear()
                .domain([0, d3.max(data, d => d.value)])
                .range([height - margin.bottom, margin.top])
            let x = d3.scaleUtc()
                .domain(d3.extent(data, d => d.date))
                .range([margin.left, width - margin.right])
            let area = (x, y) => d3.area()
                .defined(d => !isNaN(d.value))
                .x(d => x(d.date))
                .y0(y(0))
                .y1(d => y(d.value))

        },
        calcMonth(startTime, endTime) {
            let year = parseInt(endTime / 100) - parseInt(startTime / 100);
            let month = endTime % 100 - startTime % 100 + 1;
            let sumMonth = year * 12 + month;
            return sumMonth;
        },
        calcTimeScale(data) {
            let startTime = 9999999;
            let endTime = 0;
            let maeMax = 0;
            for (const d of data) {
                for (const sub_d of d['sub slice']) {
                    startTime = Math.min(startTime, parseInt(sub_d['train_begin']));
                    endTime = Math.max(endTime, parseInt(sub_d['test_end']));
                    maeMax = Math.max(maeMax, d['MAE']);
                }
            }
            let month = this.calcMonth(startTime, endTime);
            return [startTime, scaleLinear([0, month], [0, this.elWidth]), scaleLinear([0, maeMax], [1, 0])];
        },
        calcTimeData(data) {
            let r_data = new Array();
            let t_data = new Object();
            for (const d of data['sub slice']) {
                r_data.push({
                    train: {
                        x: this.timeScale(this.calcMonth(this.startTime, d['train_begin'])),
                        w: this.timeScale(this.calcMonth(d['train_begin'], d['train_end'])),
                        fill: 'steelblue'
                    },
                    test: {
                        x1: this.timeScale(this.calcMonth(this.startTime, d['test_begin'])),
                        x2: this.timeScale(this.calcMonth(this.startTime, d['test_end'])),
                        w: this.timeScale(this.calcMonth(d['test_begin'], d['test_end'])),
                        fill: interpolateRdBu(Math.random())
                    }
                })
            }
            t_data = {
                slice: data['slice_info']['slice_number'] + '-slice',
                smooth: '13-Average',
                bar_data: r_data.reverse()
            }
            return t_data;
        }
    },
    created() {
    },
    mounted() {
        this.elHeight = this.$refs.DataTransformation.offsetHeight;
        this.elWidth = this.$refs.DataTransformation.offsetWidth;
        this.tlHeight = this.$refs.timeline.offsetHeight;
        this.tlWidth = this.$refs.timeline.offsetWidth;
        // this.d = this.calcCurve();
        // // console.log(this.timeData)
        // console.log(this.sliceData);
        const dataStore = useDataStore();
        dataStore.$subscribe((mutation, state) => {
            [this.startTime, this.timeScale, this.maeScale] = this.calcTimeScale(this.sliceData);
            let barData = new Array();
            for (const d of this.sliceData) {
                barData.push(this.calcTimeData(d));
            }
            // console.log(barData);
            this.barData = barData;
        })
        // [this.startTime, this.timeScale, this.maeScale] = this.calcTimeScale(this.sliceData);
        // let barData = new Array();
        // for (const d of this.sliceData) {
        //     barData.push(this.calcTimeData(d));
        // }
        // // console.log(barData);
        // this.barData = barData;
    },
}
</script>
<style scoped>
*,
*::before,
*::after {
    font-weight: normal;
}

#DataTransformation {
    font-size: 20px;
    /* font-family: Roboto; */
}
</style>
