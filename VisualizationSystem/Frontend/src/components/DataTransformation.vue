<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-02-20 10:12:00
-->
<template>
    <div class="frameworkTitle" style="padding-right: 10px;">
        <div class="title">Data Transformation Overview</div>
        <p class="titleTriangle"></p>
        <div style="float: right; margin-top: 3px;">
            <span>Metric: </span>
            <el-select v-model="heatTag" class="m-2" placeholder="Select" style="width: 150px;">
                <el-option v-for="item in heatOptions" :key="item" :label="item" :value="item" />
            </el-select>
        </div>
    </div>
    <div class="frameworkBody">
        <div ref="DataTransformation" style="height: calc(50%); width: 100%;">
            <!-- <el-table :data="tableData" style="width: 100%" height="100%"
                                                                            :header-cell-style="{ 'text-align': 'center', 'font-size': '16px', 'background-color': 'rgba(250, 250, 250, 1)' }"
                                                                            :cell-style="{ 'text-align': 'center', 'backgro und-color': 'rgba(250, 250, 250, 1)', 'font-size': '16px' }">
                                                                            <el-table-column type="expand">
                                                                                <template #default="props">
                                                                                    <div m="4">
                                                                                        <el-table :data="props.row['bar_data']" stripe style="width: 100%; float: right;"
                                                                                            :table-layout="'auto'" :header-cell-style="{ 'text-align': 'center' }"
                                                                                            :cell-style="{ 'text-align': 'center' }">
                                                                                            <el-table-column label="ID" prop="id" width="50" />
                                                                                            <el-table-column label="BEGIN" prop="startTime" width="120" />
                                                                                            <el-table-column label="END" prop="endTime" width="110" />
                                                                                            <el-table-column label="TIME SLICE">
                                                                                                <template #default="d">
                                                                                                    <svg height="30" width="100%">
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
                                                                        </el-table> -->

            <!-- <el-table :data="heatRectData" style="width: 100%" border height="100%"
                                                                        :header-cell-style="{ 'text-align': 'center', 'font-size': '16px', 'background-color': 'rgba(250, 250, 250, 1)' }"
                                                                        :cell-style="{ 'text-align': 'center', 'background-color': 'rgba(250, 250, 250, 1)', 'font-size': '16px' }">
                                                                        <el-table-column label="Heatmap" prop="heat_data">
                                                                            <template #default="scope">
                                                                                <svg height="30" width="100%">
                                                                                    <rect v-for="(item, item_i) in scope.row['heat']" :key="'heat_' + item_i" :x="item.x" :y="0"
                                                                                        :width="item.w" :height="50" :fill="item.color">
                                                                                    </rect>
                                                                                </svg>
                                                                            </template>
                                                                        </el-table-column>
                                                                    </el-table> -->


            <svg height="100%" width="100%">
                <g v-for="(item, i) in heatRectData" :key="'heat_g' + i" :transform="translate(0, item.h * i, 0)">
                    <rect v-for="(item_h, item_i) in item['heat']" :key="'heat_' + item_i" :x="item_h.x" :y="0"
                        :width="item_h.w" :height="item_h.h" :fill="colorScale(item_h.raw, item_h.error, heatTag)"
                        @click="timeCompare(item.tag)">
                    </rect>
                </g>
                <g v-for="(item, i) in groupPath" :key="'group_g' + i" :transform="translate(0, 0, 0)">
                    <path :d="'M ' + item.x1 + ' ' + item.y1 + ' L ' + item.x2 + ' ' + item.y2" fill="none" stroke="black">
                    </path>
                </g>

            </svg>


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
        <div ref="timeline" style="height: calc(50% - 15px); width: 100%; margin-top: 15px;">
            <svg id="timeline" height="100%" width="100%">
                <!-- <g>
                                                                                <g v-for="(item, i) in heatRectData" :key="'r_g' + i" :transform="translate(0, 40 * i, 0)">
                                                                                    <rect v-for="(r_item, r_i) in item" :key="'h_r' + r_i" :x="r_item.x" :y="3" :height="30"
                                                                                        :width="r_item.w" :fill="r_item.color" @click="timeCompare(parseInt(i / 3))"></rect>
                                                                                    <text x="0" y="20">{{ item[0].t }}</text>
                                                                                </g>
                                                                            </g> -->
                <g :transform="translate(0, 0, 0)" id="timeline_g">
                    <g id="raw_line_g" :transform="translate(0, 110, 0)">
                        <!-- <g v-for="(item, i) in sparkboxData" :key="'box' + i"> -->
                        <!-- <rect :x="item.rect1.x" :y="item.rect1.y" :width="item.rect1.w" :height="item.rect1.h"
                                                                                            fill="#f2f5fa"></rect> -->
                        <!-- <rect :x="item.rect2.x" :y="item.rect2.y" :width="item.rect2.w" :height="item.rect2.h"  fill="#dce3f3"></rect> -->
                        <!-- <path
                                                                                            :d="'M ' + item.line.x1 + ' ' + item.line.y + ' L ' + item.line.x2 + ' ' + item.line.y"
                                                                                            :fill="'none'" :stroke="'#6d70b6'" stroke-width="3"></path> -->
                        <!-- </g> -->
                        <defs>
                            <clipPath id="clipPath">
                                <rect :x="50" :y="20" :width="tlWidth - 50" :height="tlHeight - 50"></rect>
                            </clipPath>
                        </defs>
                        <g id="brush_path_g" clip-path="url(#clipPath)">
                            <!-- <path :d="rawTimeLineData" stroke="steelblue" :fill="'none'"></path> -->
                        </g>
                        <g :transform="translate(0, tlHeight - 130, 0)">
                            <path :d="'M ' + 50 + ' 0 ' + 'L ' + (tlWidth - 0) + ' 0'" :fill="'none'" stroke="black">
                            </path>
                            <g v-for="(item, i) in timeAxis" :key="'xa' + i">
                                <path :d="'M ' + item.x + ' 0 ' + 'L ' + item.x + ' 5'" :fill="'none'" stroke="black">
                                </path>
                                <text :x="item.x" y="20" font-size="12" text-anchor="middle">{{ item.text }}</text>
                            </g>
                        </g>
                        <g>
                            <path :d="smoothTimeLineData" stroke="red" :fill="'none'"></path>
                            <!-- <path v-for="(item, i) in selectAverageLine" :key="'xb' + i"
                                                                                            :d="'M ' + item.x1 + ' ' + item.y + ' L ' + item.x2 + ' ' + item.y" :fill="'none'"
                                                                                            :stroke="'orange'" stroke-width="3"></path> -->
                        </g>
                    </g>
                    <g id="brush_g" :transform="translate(0, -15, 0)"></g>
                    <g :transform="translate(0, -15, 0)">
                        <path :d="brushTimeLineData" stroke="steelblue" :fill="'none'"></path>
                        <g id="brush_path_line"></g>
                        <g id="" :transform="translate(0, 100, 0)">
                            <!-- <path :d="'M ' + 50 + ' 0 ' + 'L ' + (tlWidth - 0) + ' 0'" :fill="'none'" stroke="black">
                                        </path> -->
                            <g v-for="(item, i) in brushTimeAxis" :key="'xa' + i">
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
import SN_raw_data from "../assets/SN_m_tot_V2.0.csv";
import sr3 from '../assets/data/SN_rolling3_tot.csv';
import sr6 from '../assets/data/SN_rolling6_tot.csv';
import sr9 from '../assets/data/SN_rolling9_tot.csv';
import sr13 from '../assets/data/SN_rolling13_tot.csv';
import sr26 from '../assets/data/SN_rolling26_tot.csv';
import sa3 from '../assets/data/SN_weighted_moving_average3_tot.csv';
import sa6 from '../assets/data/SN_weighted_moving_average6_tot.csv';
import sa9 from '../assets/data/SN_weighted_moving_average9_tot.csv';
import sa13 from '../assets/data/SN_weighted_moving_average13_tot.csv';
import sa26 from '../assets/data/SN_weighted_moving_average26_tot.csv';
// import SN_rolling_6_data from "../assets/SN_rolling6_tot.csv";
// import SN_rolling_13_data from "../assets/SN_weighted_moving_average13_tot.csv";
import { select } from 'd3-selection';
import { extent, max, min, sum } from 'd3-array';
import { brushX } from 'd3-brush';
import * as vsup from 'vsup';
export default {
    name: 'DataTransformationView',
    props: ['timeData', 'sliceData'],
    data () {
        return {
            elHeight: 1000,
            elWidth: 1000,
            tlHeight: 100,
            tlWidth: 100,
            heatHeight: 0,
            heatTag: 'Raw + RMSE',
            heatOptions: ['Raw + RMSE', 'Raw', 'RMSE'],
            sample: ['10-slice', '7-slice', '3-slice'],
            smooth: ['Raw Sequence', 'N-Average', 'EMA/Holt'],
            d: [],
            startTime: 0,
            timeScale: null,
            maeScale: null,
            timeSliceData: [],
            barData: [],
            tableData: [],
            rawTimeLineData: 'M 0 0',
            brushTimeLineData: 'M 0 0',
            smoothTimeLineData: 'M 0 0',
            timeAxis: [],
            brushTimeAxis: [],
            sparkboxData: [],
            heatRectData: [],
            selectAverageLine: [],
            scaleRange: {
                raw: [99999, -999999],
                error: [99999, -999999]
            },
            groupPath: [],
            smoothData: [],
            select_smooth_data: [],
            select_time_step: [0, 2459],
            lineGenerateFunc: null,
            yScale: null,
            xScale: null,
            rxScale: null,
            lineData: [],
            smoothLineData: [],
            smoothTag: 0
        }
    },
    methods: {
        colorScale: function (raw_value, error_value, tag) {
            let rawRange = this.scaleRange.raw;
            let errorRange = this.scaleRange.error;

            let heatColor = interpolateYlOrRd;
            // let heatColor = interpolateRdBu;
            let rawDataScale = scaleLinear(rawRange, [0, 1]);
            let errorDataScale = scaleLinear(errorRange, [0, 1]);
            let quantization = vsup.quantization().branching(2).layers(4).valueDomain(rawRange).uncertaintyDomain(errorRange);
            let heatScale = vsup.scale().quantize(quantization).range(heatColor);

            if (tag == "Raw + RMSE") {
                return heatScale(raw_value, error_value);
            }
            else if (tag == "Raw") {
                return heatColor(rawDataScale(raw_value, error_value));
            }
            else if (tag == "RMSE") {
                return heatColor(errorDataScale(raw_value, error_value));
            }
        },
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
            let margin = ({ top: 20, right: 0, bottom: 30, left: 30 });
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
            for (let i = 0; i < data.length; i += 10) {
                // let tempValue = Array.from(new Set(data.slice(i, i + 10).map(d => parseFloat(d.value)).sort((a, b) => a - b)));
                let tempValue = data.slice(i, i + 10).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                let sumData = sum(tempValue);

                // console.log(sumData)

                // console.log(tempValue[tempValue.length /2 - 1], tempValue.length /2 - 1);
                sparkboxData.push({
                    rect1: {
                        x: x(parseInt(data[i].id)),
                        y: y(tempValue[tempValue.length - 1]),
                        w: Math.abs(x(parseInt(data[i + ((i + 10 < data.length) ? 10 : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                        h: Math.abs(y(tempValue[0]) - y(tempValue[tempValue.length - 1]))
                    },
                    rect2: {
                        x: x(parseInt(data[i].id)),
                        y: y(tempValue[parseInt(tempValue.length * 3 / 4) - 1]),
                        w: Math.abs(x(parseInt(data[i + ((i + 10 < data.length) ? 10 : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                        h: Math.abs(y(tempValue[parseInt(tempValue.length * 3 / 4) - 1]) - y(tempValue[parseInt(tempValue.length / 4) - 1]))
                    },
                    line: {
                        x1: x(parseInt(data[i].id)),
                        // y: y(tempValue[parseInt(tempValue.length /2) - 1]),
                        // y: y((tempValue[0] + tempValue[parseInt(tempValue.length - 1)]) / 2),
                        y: y(sumData / tempValue.length),
                        x2: Math.abs(x(parseInt(data[i + ((i + 10 < data.length) ? 10 : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))) + x(parseInt(data[i].id))
                    }
                })
            }
            // console.log(sparkboxData)
            return sparkboxData;
        },
        setupBrush: function (data) {
            let width = this.tlWidth;
            let height = this.tlHeight;
            let focusHeight = 100;
            let margin = ({ top: 20, right: 0, bottom: 30, left: 50 });
            const timeBrush = brushX()
                .extent([[margin.left, margin.top], [width - margin.right, focusHeight]])
                .on('brush', brushed)
                .on('end', brushed);
            let x = scaleLinear()
                .domain([0, max(data, d => parseInt(d.id))])
                .range([margin.left, width - margin.right])

            let _this = this;

            function brushed ({ selection }) {
                // console.log(selection);
                let timeStep = [parseInt(_this.rxScale(selection[0])), parseInt(_this.rxScale(selection[1]))];
                // let maxY = max(data, (d, i) => {
                //     if (i >= timeStep[0] && i <= timeStep[1]) {
                //     return parseFloat(d.value)
                //     }
                //     return 0;
                // });


                _this.xScale.domain(timeStep);
                // _this.yScale.domain([0, maxY]);
                let xAxis = [];
                for (let ii = timeStep[0]; ii <= timeStep[1]; ii += (timeStep[1] - timeStep[0]) / 10) {
                    // console.log(i, _this.lineData);
                    let i = parseInt(ii);
                    xAxis.push({
                        x: _this.xScale(i),
                        y: 0,
                        text: parseInt(parseInt(_this.lineData[i].timestamp) / 100) + '.' + parseInt(parseInt(_this.lineData[i].timestamp) % 100)
                    });

                }
                // _this.timeAxis = xAxis;

                let lineGenerate = line()
                    .x(d => _this.xScale(d.id))
                    .y(d => _this.yScale(d.value));

                _this.select_time_step = timeStep;

                select('#time_path_raw').attr('d', lineGenerate(_this.lineData));
                if (_this.smoothTag) {

                    select('#time_path_selected').attr('d', lineGenerate(_this.smoothLineData));
                }
                
                select("#selected_area").attr('d', 'M ' + parseInt(selection[0]) + ' ' + 85 + ' L ' + parseInt(selection[1]) + ' ' + 85 + ' L ' + (_this.elWidth) + ' 130 L 50 130 Z')

                // console.log(timeStep);
                // _this.calcTimeLineCompare(data, [], _this.tlHeight, _this.tlWidth, _this.select_time_step)
                // let d = _this.calcTimeLineCompare(SN_raw_data, [], _this.tlHeight, _this.tlWidth, _this.select_time_step);

                // _this.rawTimeLineData = d[0];
                // _this.smoothTimeLineData = d[1];
            }
            select('#brush_g').call(timeBrush)
                .call(timeBrush.move, [x(988), x(1760)]);
        },
        calcTimeLine (data, height, width) {
            let margin = ({ top: 20, right: 0, bottom: 30, left: 50 });
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
            const rx = scaleLinear()
                .domain([margin.left, width - margin.right])
                .range([0, max(data, d => parseInt(d.id))]);
            this.xScale = x;
            this.yScale = y;
            this.rxScale = rx;
            // console.log(y.domain(), x.domain(), data);

            // let area = (x, y) => d3.area()
            //     .defined(d => !isNaN(d.value))
            //     .x(d => x(d.date))
            //     .y0(y(0))
            //     .y1(d => y(d.value))
            let lineGenerate = line()
                .x(d => x(d.id))
                .y(d => y(d.value));
            this.lineGenerateFunc = lineGenerate;

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
            for (let i = 0; i < data.length; i += parseInt(data.length / 10)) {
                timeAxis.push({
                    x: x(i),
                    text: parseInt(parseInt(data[i].timestamp) / 100) + '.' + parseInt(parseInt(data[i].timestamp) % 100)
                })
            }
            this.timeAxis = timeAxis;
            this.brushTimeAxis = timeAxis;
            // select('#timeline').append('g').call(xAxis, x, focusHeight);
            select('#raw_line_g').append('g').attr('id', 'yAxis_g').call(yAxis, y, 'value');
            // select('#brush_path_g').append("defs").append("clipPath")
            //     .attr("id", "clip")
            //     .append("rect")
            //     .attr('x', margin.left)
            //     .attr('y', margin.top)
            //     .attr("width", width - margin.right - margin.left)
            //     .attr("height", height - 100 - margin.bottom - margin.top);
            select('#brush_path_g')
                // .append('g')
                // .data([{
                //     value: data
                // }])
                // .enter()
                .append('path')
                .attr('id', 'time_path_raw')
                .attr('d', d => {
                    return lineGenerate(data)
                })
                .attr('stroke', 'steelblue')
                .attr('stroke-width', 1.5)
                .attr('fill', 'none')
                const _this = this;


            select("#timeline_g").append('path').attr('id', 'selected_area').attr('d', 'M ' + parseInt(988) + ' ' + 85 + ' L ' + parseInt(1760) + ' ' + 85 + ' L ' + (_this.elWidth) + ' 130 L 50 130 Z').attr('stroke', 'black').attr('stroke-width', 1).attr('fill', 'black').attr('opacity', 0.15);

            select('#brush_path_line')
                // .append('g')
                // .data([{
                //     value: data
                // }])
                // .enter()
                .append('path')
                .attr('id', 'time_brush')
                .attr('d', d => {
                    return lineGenerate2(data)
                })
                .attr('stroke', 'steelblue')
                .attr('stroke-width', 1)
                .attr('fill', 'none')
            this.lineData = data;

            // return [lineGenerate(data), lineGenerate2(data)];
        },
        calcTimeLineCompare (tsmooth_data) {
            this.smoothTag = 1;
            this.smoothLineData = tsmooth_data;

            let lineGenerate = line()
                .x(d => this.xScale(d.id))
                .y(d => this.yScale(d.value));
            select('#brush_path_g')
                // .append('g')
                // .data([{
                //     value: data
                // }])
                // .enter()
                .append('path')
                .attr('id', 'time_path_selected')
                .attr('d', d => {
                    return lineGenerate(tsmooth_data)
                })
                .attr('stroke', 'red')
                .attr('stroke-width', 1.5)
                .attr('fill', 'none')
            // return [lineGenerate(data), lineGenerate(smooth_data), average_line];
        },
        timeCompare (select_num) {
            // console.log(select_num, this.smoothData);
            this.select_smooth_data = this.smoothData[select_num]
            this.calcTimeLineCompare(this.smoothData[select_num]);

            // this.rawTimeLineData = d[0];
            // this.smoothTimeLineData = d[1];
            // console.log(d[1], this.smoothTimeLineData);
        },
        calcMonth (startTime, endTime) {
            let year = parseInt(endTime / 100) - parseInt(startTime / 100);
            let month = endTime % 100 - startTime % 100 + 1;
            let sumMonth = year * 12 + month;
            return sumMonth;
        },
        calcHeat (raw_data, smooth_data, skipLength, width, tag) {
            let margin = ({ top: 20, right: 0, bottom: 30, left: 50 });
            let x = scaleLinear()
                .domain([0, max(raw_data, d => parseInt(d.id))])
                .range([margin.left, width - margin.right])
            let heat_data = [];
            for (let i = 0; i < raw_data.length; i += skipLength) {
                // let tempValue = Array.from(new Set(data.slice(i, i + 10).map(d => parseFloat(d.value)).sort((a, b) => a - b)));
                let rawTempValue = raw_data.slice(i, i + skipLength).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                let rawSum = sum(rawTempValue);
                let smoothTempValue = smooth_data.slice(i, i + skipLength).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                let smoothSum = sum(smoothTempValue);
                heat_data.push({
                    rawData: smoothSum / smoothTempValue.length,
                    errorData: Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length),
                    x: x(parseInt(raw_data[i].id)),
                    w: Math.abs(x(parseInt(raw_data[i + ((i + skipLength < raw_data.length) ? skipLength : (raw_data.length - 1 - i))].id)) - x(parseInt(raw_data[i].id))),
                });
            }
            let rawRange = [min(heat_data, d => d.rawData), max(heat_data, d => d.rawData)];
            let errorRange = [min(heat_data, d => d.errorData), max(heat_data, d => d.errorData)];
            // console.log(rawRange, errorRange)
            let heatRectData = [];
            let rawRectData = [];
            let errorRectData = [];
            let heatColor = interpolateYlOrRd;
            // let heatColor = interpolateRdBu;
            let rawDataScale = scaleLinear(rawRange, [0, 1]);
            let errorDataScale = scaleLinear(errorRange, [0, 1]);
            // let heatScale = scaleLinear([0, 14], [0, 1]);
            // let colorStep = [1, 2, 4, 8];

            let quantization = vsup.quantization().branching(2).layers(4).valueDomain(rawRange).uncertaintyDomain(errorRange);
            let heatScale = vsup.scale().quantize(quantization).range(heatColor);
            for (let i in heat_data) {
                let hd = heat_data[i];
                // let errorScale = parseFloat((hd.errorRange - errorRange[0]) / (errorRange[1] - errorRange[0]));
                // let errorStep = ((errorScale * 4 == parseInt(errorScale * 4)) && errorScale != 0) ? parseInt(errorScale * 4) - 1 : parseInt(errorScale * 4);
                // let rawScale = parseFloat((hd.rawData - rawRange[0]) / (rawRange[1] - rawRange[0]));
                // // console.log(parseInt(rawScale * 4))
                // let colorScale = sum(colorStep.slice(0, errorStep)) + parseInt(rawScale * colorStep[errorStep]);
                // console.log(colorScale,colorStep[errorStep], errorStep, sum(colorStep.slice(0, parseInt(errorScale * 4))))
                this.scaleRange.raw[0] = Math.min(this.scaleRange.raw[0], hd.rawData);
                this.scaleRange.raw[1] = Math.max(this.scaleRange.raw[1], hd.rawData);
                this.scaleRange.error[0] = Math.min(this.scaleRange.error[0], hd.rawData);
                this.scaleRange.error[1] = Math.max(this.scaleRange.error[1], hd.rawData);

                heatRectData.push({
                    t: tag,
                    x: hd.x,
                    w: hd.w,
                    h: this.elHeight / 22 - 3,
                    // color: (heatScale(hd.rawData, hd.errorData))
                    raw: hd.rawData,
                    error: hd.errorData
                });
                rawRectData.push({
                    t: tag,
                    x: hd.x,
                    w: hd.w,
                    h: this.elHeight / 22 - 3,
                    // color: heatColor(rawDataScale(hd.rawData))
                    raw: hd.rawData,
                    error: hd.errorData

                });
                errorRectData.push({
                    t: tag,
                    x: hd.x,
                    w: hd.w,
                    h: this.elHeight / 22 - 3,
                    // color: heatColor(errorDataScale(hd.errorData))
                    raw: hd.rawData,
                    error: hd.errorData
                });
                // console.log(heatColor(heatScale((colorScale != 0 ? colorScale - 1 : 0))), colorScale, heatScale((colorScale == 0 ? colorScale - 1 : 0)))
            }
            if (tag == 'raw')
                return {
                    heat: rawRectData, raw: rawRectData, error: rawRectData, tag: tag,
                    h: this.elHeight / 22
                }
            return {
                heat: heatRectData, raw: rawRectData, error: errorRectData, tag: tag,
                h: this.elHeight / 22
            };
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
            // console.log(data['sub slice']);
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
        },
        calcGroup: function () {
            let midDot = (this.elHeight / 18);
            let left = 50;
            let groupRow = [];
            let groupColum = [];
            let tp = [];
            for (let i = 1; i <= 18; ++i) {
                groupRow.push({
                    x1: 50,
                    y1: (i) * midDot - this.elHeight / 36,
                    x2: 50 - 48 / 3,
                    y2: (i) * midDot - this.elHeight / 36
                });
                if (i % 2 == 0) {
                    groupColum.push({
                        x1: 50 - 48 / 3,
                        x2: 50 - 48 / 3,
                        y1: (i - 1) * midDot - this.elHeight / 36,
                        y2: i * midDot - this.elHeight / 36
                    })
                    groupRow.push({
                        x1: 50 - 48 / 3,
                        x2: i == 2 ? 50 - 48 * 3 / 3 : 50 - 48 * 2 / 3,
                        y1: ((i) * midDot - this.elHeight / 36 + (i - 1) * midDot - this.elHeight / 36) / 2,
                        y2: ((i) * midDot - this.elHeight / 36 + (i - 1) * midDot - this.elHeight / 36) / 2,
                    })
                }
            }
            groupColum.push({
                x1: 50 - 48 * 2 / 3,
                x2: 50 - 48 * 2 / 3,
                y1: ((4) * midDot - this.elHeight / 36 + (3) * midDot - this.elHeight / 36) / 2,
                y2: ((10) * midDot - this.elHeight / 36 + (9) * midDot - this.elHeight / 36) / 2
            })

            groupRow.push({
                x1: 50 - 48 * 2 / 3,
                x2: 50 - 48 * 3 / 3,
                y1: ((14) * midDot + (12) * midDot - 4 * this.elHeight / 36) / 4,
                y2: ((14) * midDot + (12) * midDot - 4 * this.elHeight / 36) / 4
            })

            groupRow.push({
                x1: 50 - 48 * 2 / 3,
                x2: 50 - 48 * 3 / 3,
                y1: ((30) * midDot + (28) * midDot - 4 * this.elHeight / 36) / 4,
                y2: ((30) * midDot + (28) * midDot - 4 * this.elHeight / 36) / 4
            })

            groupColum.push({
                x1: 50 - 48 * 2 / 3,
                x2: 50 - 48 * 2 / 3,
                y1: ((12) * midDot - this.elHeight / 36 + (11) * midDot - this.elHeight / 36) / 2,
                y2: ((18) * midDot - this.elHeight / 36 + (17) * midDot - this.elHeight / 36) / 2
            })

            groupColum.push({
                x1: 50 - 48 * 3 / 3,
                x2: 50 - 48 * 3 / 3,
                y1: ((2) * midDot - this.elHeight / 36 + (1) * midDot - this.elHeight / 36) / 2,
                y2: ((15) * midDot - this.elHeight / 36 + (14) * midDot - this.elHeight / 36) / 2
            })

            let group = groupRow.concat(groupColum);
            return group;
        }
    },
    created () {
    },
    mounted () {
        this.elHeight = this.$refs.DataTransformation.offsetHeight;
        this.elWidth = this.$refs.DataTransformation.offsetWidth;
        this.tlHeight = this.$refs.timeline.offsetHeight * 1;
        this.heatHeight = this.$refs.timeline.offsetHeight * 0.3;
        this.tlWidth = this.$refs.timeline.offsetWidth;
        // this.d = this.calcCurve();
        // // console.log(this.timeData)
        // console.log(this.sliceData);
        // const dataStore = useDataStore();
        // dataStore.$subscribe((mutation, state) => {
        // [this.startTime, this.timeScale, this.maeScale] = this.calcTimeScale(this.sliceData);
        // let barData = new Array();
        // for (const d of this.sliceData) {
        //     barData.push(this.calcTimeData(d));
        // }

        // this.tableData = barData;

        // this.sparkboxData = this.calcSparkBox(SN_row_data, this.tlHeight, this.tlWidth);

        this.calcTimeLine(SN_raw_data, this.tlHeight, this.tlWidth);

        // this.heatRectData = this.heatRectData.concat(this.calcHeat(SN_row_data, SN_rolling_6_data, this.heatHeight, this.tlWidth, 6));

        // this.heatRectData = this.heatRectData.concat(this.calcHeat(SN_row_data, SN_rolling_13_data, this.heatHeight, this.tlWidth, 13))

        let smoothData = { raw: SN_raw_data, sr3: sr3, sr6: sr6, sr9: sr9, sr13: sr13, sr26: sr26, sa3: sa3, sa6: sa6, sa9: sa9, sa13: sa13, sa26: sa26 };
        this.smoothData = smoothData;
        for (let d in smoothData) {
            for (let sk = 1; sk <= 13; sk += 12) {
                this.heatRectData.push(this.calcHeat(SN_raw_data, smoothData[d], sk, this.tlWidth, d));
            }
        }

        this.setupBrush(SN_raw_data);

        // console.log(this.heatRectData);
        // this.groupPath = this.calcGroup();


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
<style>
*,
*::before,
*::after {
    font-weight: normal;
}

#DataTransformation {
    font-size: 20px;
    /* font-family: Roboto; */
}

.el-table .el-table__cell {
    padding: 0px;
}
</style>
