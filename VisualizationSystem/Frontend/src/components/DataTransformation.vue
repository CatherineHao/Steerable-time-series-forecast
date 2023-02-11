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
        <div ref="DataTransformation" style="height: calc(55%); width: 100%; overflow-y: auto;">
            <el-table :data="tableData" style="width: 100%" height="100%"
                :header-cell-style="{ 'text-align': 'center', 'font-size': '16px', 'background-color': 'rgba(250, 250, 250, 1)' }"
                :cell-style="{ 'text-align': 'center', 'backgro und-color': 'rgba(250, 250, 250, 1)', 'font-size': '16px' }">
                <el-table-column type="expand">
                    <template #default="props">
                        <div m="4">
                            <el-table :data="props.row['bar_data']" stripe style="width: 100%; float: right;"
                                :table-layout="'auto'" :header-cell-style="{ 'text-align': 'center' }"
                                :cell-style="{ 'text-align': 'center' }">
                                <!-- <el-table-column label="ID" prop="slice_num" />
                                <el-table-column label="MAE" prop="MAE" />
                                <el-table-column label="STD" prop="STD" />
                                <el-table-column label="MEAN" prop="Mean" /> -->
                                <el-table-column label="ID" prop="id" width="50" />
                                <el-table-column label="BEGIN" prop="startTime" width="120" />
                                <el-table-column label="END" prop="endTime" width="110" />
                                <el-table-column label="TIME SLICE">
                                    <template #default="d">
                                        <svg height="30" width="100%">
                                            <!-- <g></g> -->
                                            <rect :x="d.row.train.x" :y="5" :height="20" :width="d.row.train.w"
                                                :fill="d.row.train.fill" :opacity="0.3">
                                            </rect>
                                            <path
                                                :d="'M ' + d.row.test.x1 + ' 5 L ' + d.row.test.x1 + ' 25 L ' + d.row.test.x2 + ' 20 L ' + d.row.test.x2 + ' 10 Z'"
                                                :fill="d.row.test.fill" :stroke="'black'"></path>
                                        </svg>
                                    </template>
                                </el-table-column>
                            </el-table>
                        </div>
                    </template>
                </el-table-column>
                <el-table-column label="Slice number" prop="slice" width="120" />
                <el-table-column label="Smooth" prop="smooth" width="110" />
                <el-table-column label="Heatmap" prop="heat_data">
                    <template #default="scope">
                        <svg height="30" width="100%">
                            <rect v-for="(item, item_i) in scope.row.heat_data" :key="'heat_' + item_i"
                                :x="(elWidth - 250) / 20 * item_i" :y="8" :width="(elWidth - 250) / 20" :height="20"
                                :fill="item.fill"></rect>
                        </svg>
                    </template>
                </el-table-column>
            </el-table>


            <!-- <svg id="DataTransformation" height="100%" width="100%">
                <!~~ Transformation Tree ~~>

                <!~~ <g>
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
                </g> ~~>
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
            </svg> -->
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
import SN_row_data from "../assets/SN_m_tot_V2.0.csv";
import { select } from 'd3-selection';
import { extent, max } from 'd3-array';
export default {
    name: 'DataTransformationView',
    props: ['timeData', 'sliceData'],
    data () {
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
            barData: [],
            tableData: []
        }
    },
    methods: {
        translate (x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        },
        calcCurve () {
            let p1 = [[this.elWidth / 2, 75], [this.elWidth / 2 - this.elWidth / 3, 75 + (this.elHeight - 75 * 3) / 2]];
            let p2 = [[this.elWidth / 2, 75], [this.elWidth / 2 + this.elWidth / 3, 75 + (this.elHeight - 75 * 3) / 2]];
            let p3 = [[this.elWidth / 2 - this.elWidth / 3, -((this.elHeight - 75 * 3) / 2)], [(this.elWidth - 40) / 18, 0]];
            let p4 = [[this.elWidth / 2 - this.elWidth / 3, -(this.elHeight - 75 * 3) / 2], [2 * (this.elWidth - 40) / 9 + (this.elWidth - 40) / 18 + 10, 0]];
            let cline = line().curve(curveBumpY);
            let d = [cline(p1), cline(p2), cline(p3), cline(p4)];
            return d;
        },
        calcTimeLine (data) {
            let margin = ({ top: 20, right: 20, bottom: 30, left: 40 });
            let height = 440;
            let width = 1000;
            let focusHeight = 300;

            let y = scaleLinear()
                .domain([0, max(data, d => d.value)])
                .range([height - margin.bottom, margin.top])
            let x = scaleUtc()
                .domain(extent(data, d => d.timestamp))
                .range([margin.left, width - margin.right])
            // let area = (x, y) => d3.area()
            //     .defined(d => !isNaN(d.value))
            //     .x(d => x(d.date))
            //     .y0(y(0))
            //     .y1(d => y(d.value))

            let yAxis = (g, y, title) => g
                .attr("transform", `translate(${margin.left},0)`)
                .call(axisLeft(y))
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
                .call(axisBottom(x).ticks(width / 80).tickSizeOuter(0));
            select('#timeline').append('g').call(xAxis, x, focusHeight);
            select('#timeline').append('g').call(yAxis, y, 'value');


        },
        calcMonth (startTime, endTime) {
            let year = parseInt(endTime / 100) - parseInt(startTime / 100);
            let month = endTime % 100 - startTime % 100 + 1;
            let sumMonth = year * 12 + month;
            return sumMonth;
        },
        calcTimeScale (data) {
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
            return [startTime, scaleLinear([0, month], [0, this.elWidth - 300]), scaleLinear([0, maeMax], [1, 0])];
        },
        calcTimeData (data) {
            let r_data = new Array();
            let t_data = new Object();
            let cnt = 0;

            for (const d of data['sub slice']) {
                r_data.push({
                    id: cnt++,
                    startTime: d['train_begin'],
                    endTime: d['test_end'],
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

            let h_data = new Array();
            for (let i = 0; i < 20; ++i) {
                h_data.push({
                    fill: interpolateRdBu(Math.random())
                })
            }
            t_data = {
                slice: data['slice_info']['slice_number'] + '-slice',
                smooth: '13-Average',
                heat_data: h_data,
                // bar_data: r_data.reverse(),
                bar_data: r_data
            }
            return t_data;
        }
    },
    created () {
    },
    mounted () {
        this.elHeight = this.$refs.DataTransformation.offsetHeight;
        this.elWidth = this.$refs.DataTransformation.offsetWidth;
        this.tlHeight = this.$refs.timeline.offsetHeight;
        this.tlWidth = this.$refs.timeline.offsetWidth;
        // this.d = this.calcCurve();
        // // console.log(this.timeData)
        // console.log(this.sliceData);
        const dataStore = useDataStore();
        // dataStore.$subscribe((mutation, state) => {
        [this.startTime, this.timeScale, this.maeScale] = this.calcTimeScale(this.sliceData);
        let barData = new Array();
        for (const d of this.sliceData) {
            barData.push(this.calcTimeData(d));
        }

        this.tableData = barData;

        this.calcTimeLine(SN_row_data);
        // })


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
