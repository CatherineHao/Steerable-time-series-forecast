<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-01-12 04:24:33
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Data Transformation Overview</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="DataTransformation" style="height: calc(45%); width: 100%; ">
            <svg id="DataTransformation" height="100%" width="100%">
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
            </svg>
        </div>
        <div ref="timeline" style="height: calc(55% - 15px); width: 100%; margin-top: 15px;">
            <svg id="timeline" height="100%" width="100%">

            </svg>
        </div>
    </div>
</template>
<script>
import { curveBumpY, line } from 'd3-shape';
import { scaleUtc, scaleLinear } from 'd3-scale';
import { axisLeft, axisBottom } from 'd3-axis';
export default {
    name: 'DataTransformationView',
    props: ['timeData'],
    data() {
        return {
            elHeight: 0,
            elWidth: 0,
            tlHeight: 0,
            tlWidth: 0,
            sample: ['10-slice', '7-slice', '3-slice'],
            smooth: ['Raw Sequence', 'N-Average', 'EMA/Holt'],
            d: []
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
            
        }
    },
    created() {
    },
    mounted() {
        this.elHeight = this.$refs.DataTransformation.offsetHeight;
        this.elWidth = this.$refs.DataTransformation.offsetWidth;
        this.tlHeight = this.$refs.timeline.offsetHeight;
        this.tlWidth = this.$refs.timeline.offsetWidth;
        this.d = this.calcCurve();
        console.log(this.timeData)
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
