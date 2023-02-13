<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-02-13 09:26:36
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Data Transformation Overview</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="DataTransformation" style="height: calc(30%); width: 100%; overflow-y: auto;">
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
                            <rect v-for="(item, item_i) in scope.row.bar_data" :key="'heat_' + item_i"
                                :x="(elWidth - 250) / scope.row.slice_num * item_i" :y="8"
                                :width="(elWidth - 250) / scope.row.slice_num" :height="20" :fill="item.test.fill">
                            </rect>
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
        <div ref="timeline" style="height: calc(70% - 15px); width: 100%; margin-top: 15px;">
            <svg id="timeline" height="100%" width="100%">
                <g>
                    <g v-for="(item, i) in heatRectData" :key="'r_g' + i" :transform="translate(0, 40 * i, 0)">
                        <rect v-for="(r_item, r_i) in item" :key="'h_r' + r_i" :x="r_item.x" :y="3" :height="30"
                            :width="r_item.w" :fill="r_item.color" @click="timeCompare(parseInt(i / 3))"></rect>
                        <text x="0" y="20">{{ item[0].t }}</text>
                    </g>
                </g>
                <g :transform="translate(0, heatHeight, 0)" id="timeline_g">
                    <g>
                        <g v-for="(item, i) in sparkboxData" :key="'box' + i">
                            <rect :x="item.rect1.x" :y="item.rect1.y" :width="item.rect1.w" :height="item.rect1.h"
                                fill="#f2f5fa"></rect>
                            <!-- <rect :x="item.rect2.x" :y="item.rect2.y" :width="item.rect2.w" :height="item.rect2.h"  fill="#dce3f3"></rect> -->
                            <path
                                :d="'M ' + item.line.x1 + ' ' + item.line.y + ' L ' + item.line.x2 + ' ' + item.line.y"
                                :fill="'none'" :stroke="'#6d70b6'" stroke-width="3"></path>
                        </g>
                        <path :d="rawTimeLineData" stroke="#909196" :fill="'none'"></path>
                        <g :transform="translate(0, tlHeight - 130, 0)">
                            <path :d="'M ' + 30 + ' 0 ' + 'L ' + (tlWidth - 20) + ' 0'" :fill="'none'" stroke="black">
                            </path>
                            <g v-for="(item, i) in timeAxis" :key="'xa' + i">
                                <path :d="'M ' + item.x + ' 0 ' + 'L ' + item.x + ' 5'" :fill="'none'" stroke="black">
                                </path>
                                <text :x="item.x" y="20" font-size="12" text-anchor="middle">{{ item.text }}</text>
                            </g>
                        </g>
                        <g>
                            <path :d="smoothTimeLineData" stroke="red" :fill="'none'"></path>
                            <path v-for="(item, i) in selectAverageLine" :key="'xb' + i"
                                :d="'M ' + item.x1 + ' ' + item.y + ' L ' + item.x2 + ' ' + item.y" :fill="'none'"
                                :stroke="'orange'" stroke-width="3"></path>
                        </g>
                    </g>
                    <g id="brush_g" :transform="translate(0, tlHeight - 125, 0)"></g>
                    <g :transform="translate(0, tlHeight - 125, 0)">
                        <path :d="brushTimeLineData" stroke="steelblue" :fill="'none'"></path>
                        <g :transform="translate(0, 100, 0)">
                            <path :d="'M ' + 30 + ' 0 ' + 'L ' + (tlWidth - 20) + ' 0'" :fill="'none'" stroke="black">
                            </path>
                            <g v-for="(item, i) in timeAxis" :key="'xa' + i">
                                <path :d="'M ' + item.x + ' 0 ' + 'L ' + item.x + ' 5'" :fill="'none'" stroke="black">
                                </path>
                                <text :x="item.x" y="20" font-size="12" text-anchor="middle">{{ item.text }}</text>
                            </g>
                        </g>
                    </g>
                </g>

            </svg>
        </div>
    </div>
</template>
<script>
import { curveBumpY, line } from 'd3-shape';
import { scaleUtc, scaleLinear } from 'd3-scale';
import { axisLeft, axisBottom } from 'd3-axis';
import { interpolateRdBu, interpolateYlOrRd, schemeYlOrRd } from 'd3-scale-chromatic';
import { useDataStore } from "../stores/counter";
import SN_row_data from "../assets/SN_m_tot_V2.0.csv";
import SN_rolling_6_data from "../assets/SN_rolling6_tot.csv";
import SN_rolling_13_data from "../assets/SN_weighted_moving_average13_tot.csv";
import { select } from 'd3-selection';
import { extent, max, min, sum } from 'd3-array';
import { brushX } from 'd3-brush';
export default {
    name: 'DataTransformationView',
    props: ['timeData', 'sliceData'],
    data () {
        return {
            elHeight: 0,
            elWidth: 0,
            tlHeight: 0,
            tlWidth: 0,
            heatHeight: 0,
            sample: ['10-slice', '7-slice', '3-slice'],
            smooth: ['Raw Sequence', 'N-Average', 'EMA/Holt'],
            d: [],
            startTime: 0,
            timeScale: null,
            maeScale: null,
            tiemSliceData: [],
            barData: [],
            tableData: [],
            rawTimeLineData: 'M 0 0',
            brushTimeLineData: 'M 0 0',
            smoothTimeLineData: 'M 0 0',
            timeAxis: [],
            sparkboxData: [],
            heatRectData: [],
            selectAverageLine: []
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
        calcSparkBox (data, height, width) {
            let margin = ({ top: 20, right: 20, bottom: 30, left: 30 });
            // let height = 440;
            // let width = 1000;
            let focusHeight = 100;

            let y = scaleLinear()
                .domain([0, max(data, d => parseFloat(d.value))])
                .range([height - 100 - margin.bottom, margin.top])
            let y2 = scaleLinear()
                .domain([0, max(data, d => parseFloat(d.value))])
                .range([focusHeight, margin.top])
            let x = scaleLinear()
                .domain([0, max(data, d => parseInt(d.id))])
                .range([margin.left, width - margin.right])
            let sparkboxData = [];
            for (let i = 0; i < data.length; i += 130) {
                // let tempValue = Array.from(new Set(data.slice(i, i + 130).map(d => parseFloat(d.value)).sort((a, b) => a - b)));
                let tempValue = data.slice(i, i + 130).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                let sumData = sum(tempValue);

                // console.log(sumData)

                // console.log(tempValue[tempValue.length /2 - 1], tempValue.length /2 - 1);
                sparkboxData.push({
                    rect1: {
                        x: x(parseInt(data[i].id)),
                        y: y(tempValue[tempValue.length - 1]),
                        w: Math.abs(x(parseInt(data[i + ((i + 130 < data.length) ? 130 : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                        h: Math.abs(y(tempValue[0]) - y(tempValue[tempValue.length - 1]))
                    },
                    rect2: {
                        x: x(parseInt(data[i].id)),
                        y: y(tempValue[parseInt(tempValue.length * 3 / 4) - 1]),
                        w: Math.abs(x(parseInt(data[i + ((i + 130 < data.length) ? 130 : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                        h: Math.abs(y(tempValue[parseInt(tempValue.length * 3 / 4) - 1]) - y(tempValue[parseInt(tempValue.length / 4) - 1]))
                    },
                    line: {
                        x1: x(parseInt(data[i].id)),
                        // y: y(tempValue[parseInt(tempValue.length /2) - 1]),
                        // y: y((tempValue[0] + tempValue[parseInt(tempValue.length - 1)]) / 2),
                        y: y(sumData / tempValue.length),
                        x2: Math.abs(x(parseInt(data[i + ((i + 130 < data.length) ? 130 : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))) + x(parseInt(data[i].id))
                    }
                })
            }
            // console.log(sparkboxData)
            return sparkboxData;
        },
        calcTimeLine (data, height, width) {
            let margin = ({ top: 20, right: 20, bottom: 30, left: 30 });
            // let height = 440;
            // let width = 1000;
            let focusHeight = 100;

            let y = scaleLinear()
                .domain([0, max(data, d => parseFloat(d.value))])
                .range([height - 100 - margin.bottom, margin.top])
            let y2 = scaleLinear()
                .domain([0, max(data, d => parseFloat(d.value))])
                .range([focusHeight, margin.top])
            let x = scaleLinear()
                .domain([0, max(data, d => parseInt(d.id))])
                .range([margin.left, width - margin.right])

            const timeBrush = brushX()
                .extent([[margin.left, margin.top], [width - margin.right, focusHeight]])
                .on('brush', brushed)
                .on('end', brushed);

            function brushed ({ selection }) {
                // console.log(selection);

            }

            // console.log(y.domain(), x.domain(), data);

            // let area = (x, y) => d3.area()
            //     .defined(d => !isNaN(d.value))
            //     .x(d => x(d.date))
            //     .y0(y(0))
            //     .y1(d => y(d.value))
            let lineGenerate = line()
                .x(d => x(d.id))
                .y(d => y(d.value));
            let lineGenerate2 = line()
                .x(d => x(d.id))
                .y(d => y2(d.value));


            let yAxis = (g, y, title) => g
                .attr("transform", `translate(${margin.left},${0})`)
                .call(axisLeft(y))
                // .call(g => g.select(".domain").remove())
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
            let timeAxis = []
            for (let i = 0; i < data.length; i += 240) {
                timeAxis.push({
                    x: x(i),
                    text: parseInt(parseInt(data[i].timestamp) / 100) + '.' + parseInt(parseInt(data[i].timestamp) % 100)
                })
            }
            this.timeAxis = timeAxis;

            // select('#timeline').append('g').call(xAxis, x, focusHeight);
            select('#timeline_g').append('g').attr('id', 'yAxis_g').call(yAxis, y, 'value');
            select('#brush_g').call(timeBrush).call(timeBrush.move, [margin.left, width - margin.right]);



            return [lineGenerate(data), lineGenerate2(data)];

        },
        calcTimeLineCompare (data, smooth_data, height, width) {
            let margin = ({ top: 20, right: 20, bottom: 30, left: 30 });
            // let height = 440;
            // let width = 1000;
            // let focusHeight = 100;

            let y = scaleLinear()
                .domain([0, max(data, d => parseFloat(d.value))])
                .range([height - 100 - margin.bottom, margin.top])
            // let y2 = scaleLinear()
            //     .domain([0, max(data, d => parseFloat(d.value))])
            //     .range([focusHeight, margin.top])
            let x = scaleLinear()
                .domain([0, max(data, d => parseInt(d.id))])
                .range([margin.left, width - margin.right])

            // const timeBrush = brushX()
            //     .extent([[margin.left, margin.top], [width - margin.right, focusHeight]])
            //     .on('brush', brushed)
            //     .on('end', brushed);

            function brushed ({ selection }) {
                // console.log(selection);

            }

            // console.log(y.domain(), x.domain(), data);

            // let area = (x, y) => d3.area()
            //     .defined(d => !isNaN(d.value))
            //     .x(d => x(d.date))
            //     .y0(y(0))
            //     .y1(d => y(d.value))
            let lineGenerate = line()
                .x(d => x(d.id))
                .y(d => y(d.value));
            // let lineGenerate2 = line()
            //     .x(d => x(d.id))
            //     .y(d => y2(d.value));


            // let yAxis = (g, y, title) => g
            //     .attr("transform", `translate(${margin.left},${0})`)
            //     .call(axisLeft(y))
            //     // .call(g => g.select(".domain").remove())
            //     .call(g => g.selectAll(".title").data([title]).join("text")
            //         .attr("class", "title")
            //         .attr("x", -margin.left)
            //         .attr("y", 10)
            //         .attr("fill", "currentColor")
            //         .attr("text-anchor", "start")
            //         .text(title));
            // let xAxis = (g, x, height) => g
            //     .attr("transform", `translate(0,${height - margin.bottom})`)
            //     .call(axisBottom(x).ticks(width / 80).tickSizeOuter(0));
            // let timeAxis = []
            // for (let i = 0; i < data.length; i += 240) {
            //     timeAxis.push({
            //         x: x(i),
            //         text: parseInt(parseInt(data[i].timestamp) / 100) + '.' + parseInt(parseInt(data[i].timestamp) % 100)
            //     })
            // }
            // this.timeAxis = timeAxis;

            // select('#timeline').append('g').call(xAxis, x, focusHeight);
            // select('#timeline_g').append('g').attr('id', 'yAxis_g').call(yAxis, y, 'value');
            // select('#brush_g').call(timeBrush).call(timeBrush.move, [margin.left, width - margin.right]);

            let average_line = [];

            for (let i = 0; i < smooth_data.length; i += 130) {
                // let tempValue = Array.from(new Set(data.slice(i, i + 130).map(d => parseFloat(d.value)).sort((a, b) => a - b)));
                let tempValue = smooth_data.slice(i, i + 130).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                let sumData = sum(tempValue);

                average_line.push({
                    x1: x(parseInt(smooth_data[i].id)),
                    y: y(sumData / tempValue.length),
                    x2: Math.abs(x(parseInt(smooth_data[i + ((i + 130 < smooth_data.length) ? 130 : (smooth_data.length - 1 - i))].id)) - x(parseInt(smooth_data[i].id))) + x(parseInt(smooth_data[i].id))
                })

                // console.log(sumData)

                // console.log(tempValue[tempValue.length /2 - 1], tempValue.length /2 - 1);
                // sparkboxData.push({
                //     rect1: {
                //         x: x(parseInt(data[i].id)),
                //         y: y(tempValue[tempValue.length - 1]),
                //         w: Math.abs(x(parseInt(data[i + ((i + 130 < data.length) ? 130 : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                //         h: Math.abs(y(tempValue[0]) - y(tempValue[tempValue.length - 1]))
                //     },
                //     rect2: {
                //         x: x(parseInt(data[i].id)),
                //         y: y(tempValue[parseInt(tempValue.length * 3 / 4) - 1]),
                //         w: Math.abs(x(parseInt(data[i + ((i + 130 < data.length) ? 130 : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                //         h: Math.abs(y(tempValue[parseInt(tempValue.length * 3 / 4) - 1]) - y(tempValue[parseInt(tempValue.length / 4) - 1]))
                //     },
                //     line: {
                //         x1: x(parseInt(data[i].id)),
                //         // y: y(tempValue[parseInt(tempValue.length /2) - 1]),
                //         // y: y((tempValue[0] + tempValue[parseInt(tempValue.length - 1)]) / 2),
                //         y: y(sumData / tempValue.length),
                //         x2: Math.abs(x(parseInt(data[i + ((i + 130 < data.length) ? 130 : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))) + x(parseInt(data[i].id))
                //     }
                // })
            }



            return [lineGenerate(data), lineGenerate(smooth_data), average_line];

        },
        timeCompare (select_num) {
            console.log(select_num);
            if (select_num == 0) {
                [this.rawTimeLineData, this.smoothTimeLineData, this.selectAverageLine] = this.calcTimeLineCompare(SN_row_data, SN_rolling_6_data, this.tlHeight, this.tlWidth);
                // console.log(this.rawTimeLineData);

            }

            if (select_num == 1) {
                [this.rawTimeLineData, this.smoothTimeLineData, this.selectAverageLine] = this.calcTimeLineCompare(SN_row_data, SN_rolling_13_data, this.tlHeight, this.tlWidth);
            }
        },
        calcMonth (startTime, endTime) {
            let year = parseInt(endTime / 100) - parseInt(startTime / 100);
            let month = endTime % 100 - startTime % 100 + 1;
            let sumMonth = year * 12 + month;
            return sumMonth;
        },
        calcHeat (raw_data, smooth_data, height, width, tag) {
            let margin = ({ top: 20, right: 20, bottom: 30, left: 30 });
            let x = scaleLinear()
                .domain([0, max(raw_data, d => parseInt(d.id))])
                .range([margin.left, width - margin.right])
            let heat_data = [];
            for (let i = 0; i < raw_data.length; i += 130) {
                // let tempValue = Array.from(new Set(data.slice(i, i + 130).map(d => parseFloat(d.value)).sort((a, b) => a - b)));
                let rawTempValue = raw_data.slice(i, i + 130).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                let rawSum = sum(rawTempValue);
                let smoothTempValue = smooth_data.slice(i, i + 130).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                let smoothSum = sum(smoothTempValue);
                heat_data.push({
                    rawData: smoothSum / smoothTempValue.length,
                    errorRange: (rawSum / rawTempValue.length - smoothSum / smoothTempValue.length),
                    x: x(parseInt(raw_data[i].id)),
                    w: Math.abs(x(parseInt(raw_data[i + ((i + 130 < raw_data.length) ? 130 : (raw_data.length - 1 - i))].id)) - x(parseInt(raw_data[i].id))),
                });
            }
            let rawRange = [min(heat_data, d => d.rawData), max(heat_data, d => d.rawData)];
            let errorRange = [min(heat_data, d => d.errorRange), max(heat_data, d => d.errorRange)];
            // console.log(rawRange, errorRange)
            let heatRectData = [];
            let rawRectData = [];
            let errorRectData = [];
            // let heatColor = interpolateYlOrRd;
            let heatColor = interpolateRdBu;
            let rawDataScale = scaleLinear(rawRange, [0, 1]);
            let errorDataScale = scaleLinear(errorRange, [0, 1]);
            let heatScale = scaleLinear([0, 14], [0, 1]);
            let colorStep = [1, 2, 4, 8];
            for (let i in heat_data) {
                let hd = heat_data[i];
                let errorScale = parseFloat((hd.errorRange - errorRange[0]) / (errorRange[1] - errorRange[0]));
                let errorStep = ((errorScale * 4 == parseInt(errorScale * 4)) && errorScale != 0) ? parseInt(errorScale * 4) - 1 : parseInt(errorScale * 4);
                let rawScale = parseFloat((hd.rawData - rawRange[0]) / (rawRange[1] - rawRange[0]));
                // console.log(parseInt(rawScale * 4))
                let colorScale = sum(colorStep.slice(0, errorStep)) + parseInt(rawScale * colorStep[errorStep]);
                // console.log(colorScale,colorStep[errorStep], errorStep, sum(colorStep.slice(0, parseInt(errorScale * 4))))
                heatRectData.push({
                    t: 'vsup' + tag,
                    x: hd.x,
                    w: hd.w,
                    color: heatColor(heatScale((colorScale != 0 ? colorScale - 1 : 0)))
                }); 
                rawRectData.push({
                    t: 'raw' + tag,
                    x: hd.x,
                    w: hd.w,
                    color: heatColor(rawDataScale(hd.rawData))
                    
                });
                errorRectData.push({
                    t: 'error' + tag,
                    x: hd.x,
                    w: hd.w,
                    color: heatColor(errorDataScale(hd.errorRange))
                });
                // console.log(heatColor(heatScale((colorScale != 0 ? colorScale - 1 : 0))), colorScale, heatScale((colorScale == 0 ? colorScale - 1 : 0)))
            }
            return [heatRectData, rawRectData, errorRectData];
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
            console.log(data['sub slice']);
            let maeRange = [min(data['sub slice'], d => d['MAE']), max(data['sub slice'], d => d['MAE'])];
            let maeScale = scaleLinear(maeRange, [0, 1]);


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
                        fill: interpolateRdBu(maeScale(d['MAE']))
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
                slice_num: parseInt(data['slice_info']['slice_number']),
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
        this.tlHeight = this.$refs.timeline.offsetHeight * 0.7;
        this.heatHeight = this.$refs.timeline.offsetHeight * 0.3;
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
        this.sparkboxData = this.calcSparkBox(SN_row_data, this.tlHeight, this.tlWidth);

        [this.rawTimeLineData, this.brushTimeLineData] = this.calcTimeLine(SN_row_data, this.tlHeight, this.tlWidth);
        // console.log(this.timeLineData);
        let smoothData = [SN_rolling_6_data, SN_rolling_13_data]
        // this.heatRectData.push(this.calcHeat(SN_row_data, SN_rolling_6_data, this.heatHeight, this.tlWidth))
        // this.heatRectData.push(this.calcHeat(SN_row_data, SN_rolling_13_data, this.heatHeight, this.tlWidth))


        this.heatRectData = this.heatRectData.concat(this.calcHeat(SN_row_data, SN_rolling_6_data, this.heatHeight, this.tlWidth, 6));

        this.heatRectData = this.heatRectData.concat(this.calcHeat(SN_row_data, SN_rolling_13_data, this.heatHeight, this.tlWidth, 13))
        
        // console.log(this.heatRectData);

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
