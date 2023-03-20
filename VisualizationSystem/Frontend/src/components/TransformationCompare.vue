<!--
 * @Author: Qing Shi
 * @LastEditTime: 2023-03-20 09:31:42
-->
<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-03-13 11:47:05
-->
<template>
    <div class="frameworkTitle" style="padding-right: 10px;">
        <div class="title">Representation View</div>
        <p class="titleTriangle"></p>
        <div style="float: right; margin-top: 3px;">
            <span>Metric: </span>
            <el-select v-model="heatTag" class="m-2" placeholder="Select" style="width: 150px;">
                <el-option v-for="(item, i) in heatOptions" :key="item" :label="item" :value="i" />
            </el-select>
        </div>
    </div>
    <div class="frameworkBody">

    <!--<div ref="timeline" style="height: calc(45% - 15px); width: 100%; margin-top: 0px;">
        <svg id="timeline" height="100%" width="100%">
            <g :transform="translate(0, 0, 0)" id="timeline_g">
                <g id="raw_line_g" :transform="translate(0, 0, 0)">
                    <!~~ <g v-for="(item, i) in sparkboxData" :key="'box' + i">
                            <rect :x="item.rect1.x" :y="item.rect1.y" :width="item.rect1.w" :height="item.rect1.h"
                                                fill="#f2f5fa"></rect>
                                            <rect :x="item.rect2.x" :y="item.rect2.y" :width="item.rect2.w" :height="item.rect2.h"
                                                fill="#dce3f3"></rect>
                                            <path :d="'M ' + item.line.x1 + ' ' + item.line.y + ' L ' + item.line.x2 + ' ' + item.line.y"
                                                :fill="'none'" :stroke="'#6d70b6'" stroke-width="3"></path>
                                        </g> ~~>
                        <defs>
                            <clipPath id="clipPath">
                                <rect :x="50" :y="20" :width="tlWidth - 70" :height="tlHeight - 50"></rect>
                        </clipPath>
                    </defs>
                    <g id="brush_path_g" clip-path="url(#clipPath)">
                        <!~~ <path :d="rawTimeLineData" stroke="#777" :fill="'none'"></path> ~~>
                    </g>
                    <!~~                        <g :transform="translate(0, tlHeight - 130, 0)">
                            <path :d="'M ' + 50 + ' 0 ' + 'L ' + (tlWidth - 0) + ' 0'" :fill="'none'" stroke="black">
                            </path>
                                            <!~~ <g v-for="(item, i) in timeAxis" :key="'xa' + i">
                                                <path :d="'M ' + item.x + ' 0 ' + 'L ' + item.x + ' 5'" :fill="'none'" stroke="black">
                                                </path>
                                            <text :x="item.x" y="20" font-size="12" text-anchor="middle">{{ item.text }}</text>
                                        </g> ~~>
                                        </g>~~>
                        <g>
                            <path :d="smoothTimeLineData" stroke="red" :fill="'none'"></path>
                    </g>
                </g>
                <g :transform="translate(0, 440, 0)" id="focusLine_g">
                    <path :d="brushTimeLineData" stroke="#777" :fill="'none'"></path>
                    <g id="brush_path_line"></g>
                    <!~~ <g id="" :transform="translate(0, 100, 0)">
                            
                                                    <g v-for="(item, i) in brushTimeAxis" :key="'xa' + i">
                                                        <path :d="'M ' + item.x + ' 0 ' + 'L ' + item.x + ' 5'" :fill="'none'" stroke="black">
                                                        </path>
                                                        <text :x="item.x" y="20" font-size="12" text-anchor="middle">{{ item.text }}</text>
                                                    </g>
                                                </g> ~~>
                    </g>

                <g id="brush_g" :transform="translate(0, 440, 0)"></g>
            </g>
        </svg>
    </div>-->
    <div ref="DataTransformation" style="height: calc(100%); width: 100%; margin-top: 0px;">
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


        <svg height="100%" width="100%" transform="translate(0, 0)">
                <g v-for="(item, i) in heatRectData" :key="'heat_g' + i" :transform="translate(0, item.h * i, 0)"
                    @mouseenter="selectFile(i)" @mouseout="cancelFile(i)" @click="clickFile(i)">
                    <rect v-for="(item_h, item_i) in item['heat']" :key="'heat_' + item_i" :x="item_h.x" :y="0"
                        :width="item_h.w" :height="item_h.h" :fill="item_h.colorMap[heatTag]" :id="'tsr' + item_h.id_cnt"
                        :class="'wsr' + i" :fill-opacity="1">
                    </rect>
                    <rect v-for="(item_h, item_i) in item['res']" :key="'heat_' + item_i" :x="item_h.x" :y="0"
                        :width="item_h.w" :height="item_h.h" :fill="'orange'" :id="'tsr' + item_i"
                        :fill-opacity="item_h.fill_opacity">
                    </rect>

                    <!--<text font-size="14" font-family="Arial" text-anchor="start" dx="0em" dy="1em">{{
                        // filename[i].substring(0, filename[i].length - 8)
                        filename_type[i].substring(0, filename_type[i].indexOf('_'))
                    }}</text>

                    <text font-size="14" font-family="Arial" text-anchor="middle" dx="7em" dy="1em">{{
                        // filename[i].substring(0, filename[i].length - 8)
                        filename_type[i].substring(filename_type[i].indexOf('_') + 1, filename[i].length)
                    }}</text>-->
                </g>
                <g id="legend_g"></g>
                <g v-for="(item, i) in heatRectData" :key="'heat_g' + i" :transform="translate(0, item.h * i, 0)">
                    <rect :id="'rst' + i" class="rst" :x="item['heat'][0].x" :y="0" :width="elWidth - 0 - item['heat'][0].x"
                        :height="item['heat'][0].h" fill="none" stroke="orange" stroke-width="0"></rect>
                </g>
                <g v-for="(item, i) in coverRect" :key="'heat_g' + i"
                    :transform="translate(0, ((elHeight - 20) / 36) * (parseInt(item.cnt.substring(item.cnt.length - 1, item.cnt.length))) + 20, 0)">
                    <rect :id="'rst' + i" class="rst" :x="item.x" :y="0" :width="item.w" :height="item.h" :fill="item.fill"
                        stroke="none" stroke-width="0"></rect>
                </g>
                <g v-for="(item, i) in groupPath" :key="'group_g' + i" :transform="translate(0, 0, 0)">
                    <path :d="'M ' + item.x1 + ' ' + item.y1 + ' L ' + item.x2 + ' ' + item.y2" fill="none" stroke="black">
                    </path>
            </g>

        </svg>

        </div>
    </div>
</template>
<script>

import res_data from '../assets/model_skip_results.json';

import { arc, curveBumpY, line } from 'd3-shape';
import { scaleUtc, scaleLinear, scaleOrdinal } from 'd3-scale';
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

// import uni variable data
import d0 from '../assets/0316/rawdata_skip13_0.8.csv';
import d1 from '../assets/0316/rawdata_skip1_0.8.csv';
import d2 from '../assets/0316/rawdata_skip3_0.8.csv';
import d3 from '../assets/0316/rawdata_skip6_0.8.csv';
import d4 from '../assets/0316/rolling13_skip13_0.8.csv';
import d5 from '../assets/0316/rolling13_skip1_0.8.csv';
import d6 from '../assets/0316/rolling13_skip3_0.8.csv';
import d7 from '../assets/0316/rolling13_skip6_0.8.csv';
import d8 from '../assets/0316/rolling3_skip13_0.8.csv';
import d9 from '../assets/0316/rolling3_skip1_0.8.csv';
import d10 from '../assets/0316/rolling3_skip3_0.8.csv';
import d11 from '../assets/0316/rolling3_skip6_0.8.csv';
import d12 from '../assets/0316/rolling6_skip13_0.8.csv';
import d13 from '../assets/0316/rolling6_skip1_0.8.csv';
import d14 from '../assets/0316/rolling6_skip3_0.8.csv';
import d15 from '../assets/0316/rolling6_skip6_0.8.csv';
import d16 from '../assets/0316/rolling9_skip13_0.8.csv';
import d17 from '../assets/0316/rolling9_skip1_0.8.csv';
import d18 from '../assets/0316/rolling9_skip3_0.8.csv';
import d19 from '../assets/0316/rolling9_skip6_0.8.csv';
import d20 from '../assets/0316/weighted13_skip13_0.8.csv';
import d21 from '../assets/0316/weighted13_skip1_0.8.csv';
import d22 from '../assets/0316/weighted13_skip3_0.8.csv';
import d23 from '../assets/0316/weighted13_skip6_0.8.csv';
import d24 from '../assets/0316/weighted3_skip13_0.8.csv';
import d25 from '../assets/0316/weighted3_skip1_0.8.csv';
import d26 from '../assets/0316/weighted3_skip3_0.8.csv';
import d27 from '../assets/0316/weighted3_skip6_0.8.csv';
import d28 from '../assets/0316/weighted6_skip13_0.8.csv';
import d29 from '../assets/0316/weighted6_skip1_0.8.csv';
import d30 from '../assets/0316/weighted6_skip3_0.8.csv';
import d31 from '../assets/0316/weighted6_skip6_0.8.csv';
import d32 from '../assets/0316/weighted9_skip13_0.8.csv';
import d33 from '../assets/0316/weighted9_skip1_0.8.csv';
import d34 from '../assets/0316/weighted9_skip3_0.8.csv';
import d35 from '../assets/0316/weighted9_skip6_0.8.csv';
// import d0 from '../assets/explaindata/rawdata_skip13_0.8.csv';
// import d1 from '../assets/explaindata/rawdata_skip1_0.8.csv';
// import d2 from '../assets/explaindata/rawdata_skip3_0.8.csv';
// import d3 from '../assets/explaindata/rawdata_skip6_0.8.csv';
// import d4 from '../assets/explaindata/rolling13_skip13_0.8.csv';
// import d5 from '../assets/explaindata/rolling13_skip1_0.8.csv';
// import d6 from '../assets/explaindata/rolling13_skip3_0.8.csv';
// import d7 from '../assets/explaindata/rolling13_skip6_0.8.csv';
// import d8 from '../assets/explaindata/rolling3_skip13_0.8.csv';
// import d9 from '../assets/explaindata/rolling3_skip1_0.8.csv';
// import d10 from '../assets/explaindata/rolling3_skip3_0.8.csv';
// import d11 from '../assets/explaindata/rolling3_skip6_0.8.csv';
// import d12 from '../assets/explaindata/rolling6_skip13_0.8.csv';
// import d13 from '../assets/explaindata/rolling6_skip1_0.8.csv';
// import d14 from '../assets/explaindata/rolling6_skip3_0.8.csv';
// import d15 from '../assets/explaindata/rolling6_skip6_0.8.csv';
// import d16 from '../assets/explaindata/rolling9_skip13_0.8.csv';
// import d17 from '../assets/explaindata/rolling9_skip1_0.8.csv';
// import d18 from '../assets/explaindata/rolling9_skip3_0.8.csv';
// import d19 from '../assets/explaindata/rolling9_skip6_0.8.csv';
// import d20 from '../assets/explaindata/weighted13_skip13_0.8.csv';
// import d21 from '../assets/explaindata/weighted13_skip1_0.8.csv';
// import d22 from '../assets/explaindata/weighted13_skip3_0.8.csv';
// import d23 from '../assets/explaindata/weighted13_skip6_0.8.csv';
// import d24 from '../assets/explaindata/weighted3_skip13_0.8.csv';
// import d25 from '../assets/explaindata/weighted3_skip1_0.8.csv';
// import d26 from '../assets/explaindata/weighted3_skip3_0.8.csv';
// import d27 from '../assets/explaindata/weighted3_skip6_0.8.csv';
// import d28 from '../assets/explaindata/weighted6_skip13_0.8.csv';
// import d29 from '../assets/explaindata/weighted6_skip1_0.8.csv';
// import d30 from '../assets/explaindata/weighted6_skip3_0.8.csv';
// import d31 from '../assets/explaindata/weighted6_skip6_0.8.csv';
// import d32 from '../assets/explaindata/weighted9_skip13_0.8.csv';
// import d33 from '../assets/explaindata/weighted9_skip1_0.8.csv';
// import d34 from '../assets/explaindata/weighted9_skip3_0.8.csv';
// import d35 from '../assets/explaindata/weighted9_skip6_0.8.csv';

import { select, selectAll } from 'd3-selection';
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
            heatTag: 3,
            heatOptions: ['Raw + Difference', 'Raw', 'Difference', 'RMSE + Corr.', 'RMSE', 'Corr.'],
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
            skip_length: [13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6],
            columnData: ['Smooth', 'Skip', 'Train-Loss', 'Test-Loss', 'ACF', 'Window Performance'],
            filename: ['rawdata_skip13_0.8.csv',
                'rawdata_skip1_0.8.csv',
                'rawdata_skip3_0.8.csv',
                'rawdata_skip6_0.8.csv',
                'rolling13_skip13_0.8.csv',
                'rolling13_skip1_0.8.csv',
                'rolling13_skip3_0.8.csv',
                'rolling13_skip6_0.8.csv',
                'rolling3_skip13_0.8.csv',
                'rolling3_skip1_0.8.csv',
                'rolling3_skip3_0.8.csv',
                'rolling3_skip6_0.8.csv',
                'rolling6_skip13_0.8.csv',
                'rolling6_skip1_0.8.csv',
                'rolling6_skip3_0.8.csv',
                'rolling6_skip6_0.8.csv',
                'rolling9_skip13_0.8.csv',
                'rolling9_skip1_0.8.csv',
                'rolling9_skip3_0.8.csv',
                'rolling9_skip6_0.8.csv',
                'weighted13_skip13_0.8.csv',
                'weighted13_skip1_0.8.csv',
                'weighted13_skip3_0.8.csv',
                'weighted13_skip6_0.8.csv',
                'weighted3_skip13_0.8.csv',
                'weighted3_skip1_0.8.csv',
                'weighted3_skip3_0.8.csv',
                'weighted3_skip6_0.8.csv',
                'weighted6_skip13_0.8.csv',
                'weighted6_skip1_0.8.csv',
                'weighted6_skip3_0.8.csv',
                'weighted6_skip6_0.8.csv',
                'weighted9_skip13_0.8.csv',
                'weighted9_skip1_0.8.csv',
                'weighted9_skip3_0.8.csv',
                'weighted9_skip6_0.8.csv'],
            filename_type: ['RAW_13',
                'RAW_1',
                'RAW_3',
                'RAW_6',
                'MA-13_13',
                'MA-13_1',
                'MA-13_3',
                'MA-13_6',
                'MA-3_13',
                'MA-3_1',
                'MA-3_3',
                'MA-3_6',
                'MA-6_13',
                'MA-6_1',
                'MA-6_3',
                'MA-6_6',
                'MA-9_13',
                'MA-9_1',
                'MA-9_3',
                'MA-9_6',
                'WMA-13_13',
                'WMA-13_1',
                'WMA-13_3',
                'WMA-13_6',
                'WMA-3_13',
                'WMA-3_1',
                'WMA-3_3',
                'WMA-3_6',
                'WMA-6_13',
                'WMA-6_1',
                'WMA-6_3',
                'WMA-6_6',
                'WMA-9_3',
                'WMA-9_13',
                'WMA-9_1',
                'WMA-9_6'],
            smoothTag: 0,
            timeLinePath: null,
            linePathTag: 0,
            coverRect: []
        }
    },
    methods: {
        selectFile (num) {

            select('#rst' + num).attr('stroke-width', 3)
            // .attr('fill', '#777').attr('fill-opacity', 0.5);
            selectAll('.p_x').attr('opacity', (d, i) => {
                return d.id == num ? 1 : 0;
            })
        },
        clickFile (num) {
            let tdata = []
            selectAll('.corr_cir').attr('opacity', (d, i) => {
                if (d.class_name == this.filename[num].substring(0, this.filename[num].length - 8)) {
                    tdata.push(d);
                    // return 0.5;
                }
                return d.isShow? 0 : 0.5;
            }).attr('fill', (d, i) => {
                // if (d.class_name == this.filename[num].substring(0, this.filename[num].length - 8)) return 'orange';
                // else 
                return '#d9d9d9';
            })
            // console.log(tdata)
            selectAll('.corr_cir_out').remove();
            select('#scatter')
                .append('g')
                .selectAll('#res_c')
                .attr('id', 'res_c')
                .data(tdata)
                .enter()
                .append('circle')
                .attr('cx', d => d.x)
                .attr('cy', d => d.y)
                .attr('id', (d, i) => 'corr_c' + d.id_cnt)
                .attr('class', 'corr_cir_out')
                .attr('r', 3)
                .attr('stroke', 'black')
                .attr('fill', d => d.fill)
        },
        cancelFile (num) {
            select('#rst' + num).attr('stroke-width', 0);
            selectAll('.p_x').attr('opacity', 1)
            // selectAll('.corr_cir').attr('opacity', (d, i) => {
            //     // if (d.class_name == this.filename[num].substring(0, this.filename[num].length - 8)) {
            //     //     return 1;
            //     // }
            //     return 1;
            // })
        },
        translate (x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        },
        setupBrush: function (data) {
            let width = this.tlWidth;
            let height = this.tlHeight;
            let focusHeight = 100;
            let margin = ({ top: 20, right: 20, bottom: 30, left: 50 });
            const timeBrush = brushX()
                .extent([[margin.left, margin.top], [width - margin.right, focusHeight]])
                .on('start', brushStart)
                .on('brush', brushed)
                .on('end', brushEnd);
            let x = scaleLinear()
                .domain([0, max(data, d => parseInt(d.id))])
                .range([margin.left, width - margin.right])

            let _this = this;
            let radius = focusHeight / 2 - 10;
            let arcA = arc()
                .innerRadius(0)
                .outerRadius(radius)
                .startAngle(0)
                .endAngle((d, i) => i ? Math.PI : -Math.PI)
            let brushHandle = (g, s) => g
                .selectAll(".handle--custom")
                .data([{ type: "w" }, { type: "e" }])
                .join(
                    enter => enter.append("path")
                        .attr("class", "handle--custom")
                        .attr("fill", "white")
                        .attr("opacity", 1)
                        .attr("stroke", "#777")
                        .attr("stroke-width", 1)
                        .attr("cursor", "ew-resize")
                        .attr("d", 'M -5 ' + (-focusHeight / 2 + 30) + ' L -5 ' + (focusHeight / 2 - 30) + 'L 5 ' + (focusHeight / 2 - 30) + ' L 5 ' + (-focusHeight / 2 + 30) + ' Z'),
                )
                .attr("display", s === null ? "none" : null)
                .attr("transform", s === null ? null : (d, i) => `translate(${s[i]},${radius + margin.top})`)
            function brushStart () {
                selectAll('.sparkbox').remove();
            }
            function brushEnd ({ selection }) {
                _this.calcSparkBox(SN_raw_data, _this.tlHeight, _this.tlWidth);
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
                let timeRange = [];
                let lenRange = [];
                let cnt_i = 0
                for (let ii = timeStep[0]; ii <= timeStep[1]; ii += Math.floor(timeStep[1] - timeStep[0]) / 10) {
                    // console.log(i, _this.lineData);
                    let i = parseInt(ii);
                    cnt_i = i;
                    xAxis.push({
                        x: _this.xScale(i),
                        y: 0,
                        // text: parseInt(parseInt(_this.lineData[i].timestamp) / 100) + '.' + parseInt(parseInt(_this.lineData[i].timestamp) % 100)
                        text: (_this.lineData[i].timestamp)
                    });
                    timeRange.push(_this.lineData[i].timestamp);
                    lenRange.push(_this.xScale(i))
                }
                if (timeRange.length == 10) {
                    timeRange.push('');
                    lenRange.push(_this.elWidth - 20)
                }
                let tScale = scaleOrdinal(timeRange, lenRange)
                // console.log(timeRange, lenRange);
                selectAll('#axsg').remove();
                select('#focusLine_g').append('g').attr('id', 'axsg').attr("transform", "translate(0, -25)").call(axisBottom(tScale));

                // _this.timeAxis = xAxis;

                let lineGenerate = line()
                    .x(d => _this.xScale(d.id))
                    .y(d => _this.yScale(d.value));

                _this.select_time_step = timeStep;

                select('#time_path_raw').attr('d', lineGenerate(_this.lineData));
                if (_this.smoothTag) {

                    select('#time_path_selected').attr('d', lineGenerate(_this.smoothLineData));
                }
            }
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
                let timeRange = [];
                let lenRange = [];
                let cnt_i = 0
                for (let ii = timeStep[0]; ii <= timeStep[1]; ii += Math.floor(timeStep[1] - timeStep[0]) / 10) {
                    // console.log(i, _this.lineData);
                    let i = parseInt(ii);
                    cnt_i = i;
                    xAxis.push({
                        x: _this.xScale(i),
                        y: 0,
                        // text: parseInt(parseInt(_this.lineData[i].timestamp) / 100) + '.' + parseInt(parseInt(_this.lineData[i].timestamp) % 100)
                        text: (_this.lineData[i].timestamp)
                    });
                    timeRange.push(_this.lineData[i].timestamp);
                    lenRange.push(_this.xScale(i))
                }
                if (timeRange.length == 10) {
                    timeRange.push('');
                    lenRange.push(_this.elWidth - 20)
                }
                let tScale = scaleOrdinal(timeRange, lenRange)
                // console.log(timeRange, lenRange);
                selectAll('#axsg').remove();
                select('#focusLine_g').append('g').attr('id', 'axsg').attr("transform", "translate(0, -25)").call(axisBottom(tScale));

                // _this.timeAxis = xAxis;

                let lineGenerate = line()
                    .x(d => _this.xScale(d.id))
                    .y(d => _this.yScale(d.value));

                _this.select_time_step = timeStep;

                select('#time_path_raw').attr('d', lineGenerate(_this.lineData));
                if (_this.smoothTag) {

                    select('#time_path_selected').attr('d', lineGenerate(_this.smoothLineData));
                }

                select("#selected_area").attr('d', 'M ' + parseInt(selection[0]) + ' ' + 460 + ' L ' + parseInt(selection[1]) + ' ' + 460 + ' L ' + (_this.elWidth - margin.right) + ' 415 L 50 415 Z')
                select(this).call(brushHandle, selection);

                // console.log(timeStep);
                // _this.calcTimeLineCompare(data, [], _this.tlHeight, _this.tlWidth, _this.select_time_step)
                // let d = _this.calcTimeLineCompare(SN_raw_data, [], _this.tlHeight, _this.tlWidth, _this.select_time_step);

                // _this.rawTimeLineData = d[0];
                // _this.smoothTimeLineData = d[1];
            }
            select('#brush_g').call(timeBrush)
                .call(timeBrush.move, [x(988), x(1760)]);

        },
        calcRMSEHeat (data, smooth_dataSet, raw_data, width, height) {
            let margin = ({ top: 20, right: 20, bottom: 30, left: 50 });
            // let sdata = [];
            let maxRmse = -999999;
            let minRmse = 999999;
            let maxTime = -999999;
            let maxError = -999999;
            let minError = 999999;
            let maxRaw = -999999;
            let minRaw = 999999;
            let maxCorr = -999999;
            let minCorr = 999999;
            let heatDataSet = [];
            let heatBeforeDataSet = [];
            let id_cnt = 0;
            for (let kk in smooth_dataSet) {
                let heat_data = [];
                let heatBefore_data = [];
                let smooth_data = smooth_dataSet[kk];
                let skipLength = this.skip_length[parseInt(kk)];
                // console.log(smooth_data, skipLength);
                for (let i = 0; i < 840; i += (i == 0 && 840 % skipLength != 0 ? 840 % skipLength : skipLength)) {
                    let skp = (i == 0 && 840 % skipLength != 0 ? 840 % skipLength : skipLength);
                    let rawTempValue = raw_data.slice(i, i + skp).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                    let rawSum = sum(rawTempValue);
                    let smoothTempValue = smooth_data.slice(i, i + skp).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                    let smoothSum = sum(smoothTempValue);
                    heatBefore_data.push({
                        rawData: smoothSum / smoothTempValue.length,
                        errorData: Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length),
                        time: i,
                        rmse: 0,
                        corr: 0,
                        skip: skp,
                        id: parseInt(kk),
                        fill_opacity: 0,
                        id_cnt: '_1'
                        // x: x(parseInt(raw_data[i].id)),
                        // w: Math.abs(x(parseInt(raw_data[i + ((i + skipLength < raw_data.length) ? skipLength : (raw_data.length - 1 - i))].id)) - x(parseInt(raw_data[i].id))),
                    });
                    maxError = Math.max(maxError, Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length));
                    minError = Math.min(minError, Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length));
                    maxRaw = Math.max(maxRaw, smoothSum / smoothTempValue.length);
                    minRaw = Math.min(minRaw, smoothSum / smoothTempValue.length);
                }
                heatBeforeDataSet.push(heatBefore_data);
                for (let i = 840; i < raw_data.length; i += skipLength) {
                    let rawTempValue = raw_data.slice(i, i + skipLength).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                    let rawSum = sum(rawTempValue);
                    let smoothTempValue = smooth_data.slice(i, i + skipLength).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                    let smoothSum = sum(smoothTempValue);
                    heat_data.push({
                        rawData: smoothSum / smoothTempValue.length,
                        errorData: Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length),
                        time: i,
                        rmse: 0,
                        corr: 0,
                        skip: skipLength,
                        id: parseInt(kk),
                        fill_opacity: 1,
                        id_cnt: id_cnt++
                        // x: x(parseInt(raw_data[i].id)),
                        // w: Math.abs(x(parseInt(raw_data[i + ((i + skipLength < raw_data.length) ? skipLength : (raw_data.length - 1 - i))].id)) - x(parseInt(raw_data[i].id))),
                    });
                    maxError = Math.max(maxError, Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length));
                    minError = Math.min(minError, Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length));
                    maxRaw = Math.max(maxRaw, smoothSum / smoothTempValue.length);
                    minRaw = Math.min(minRaw, smoothSum / smoothTempValue.length);
                }
                heatDataSet.push(heat_data);
            }
            // console.log(heatBeforeDataSet);

            let lineData = [];
            for (let i in data) {
                let startPos = 840;
                let tp = [];
                for (let j in data[i]) {
                    // sdata.push({
                    //     id: i,
                    //     skip: this.skip_length[i],
                    //     time: j * this.skip_length[i] + startPos,
                    //     rmse: parseFloat(data[i][j]['rmse'])
                    // });
                    tp.push({
                        id: i,
                        skip: this.skip_length[i],
                        time: j * this.skip_length[i] + startPos,
                        errorData: 0,
                        rmse: parseFloat(data[i][j]['rmse']),
                        corr: parseFloat(data[i][j]['result_corr'])
                    });
                    // console.log(data[i][j]['129_pearson'])
                    maxTime = Math.max(maxTime, j * this.skip_length[i] + startPos);
                    maxRmse = Math.max(maxRmse, parseFloat(data[i][j]['rmse']));
                    minRmse = Math.min(minRmse, parseFloat(data[i][j]['rmse']));
                    maxCorr = Math.max(maxCorr, parseFloat(data[i][j]['result_corr']));
                    if (parseFloat(data[i][j]['result_corr']) != 0)
                        minCorr = Math.min(minCorr, parseFloat(data[i][j]['result_corr']));
                }
                lineData.push(tp);
            }
            // console.log(heatDataSet)
            // console.log(lineData);
            // console.log(maxCorr, minCorr);


            for (let i in heatDataSet) {
                for (let j in heatDataSet[i]) {
                    heatDataSet[i][j]['rmse'] = lineData[parseInt(i)][j]['rmse'];
                    heatDataSet[i][j]['corr'] = lineData[parseInt(i)][j]['corr'];
                    heatBeforeDataSet[i].push(heatDataSet[i][j])
                }
                heatBeforeDataSet[i] = heatBeforeDataSet[i].concat(heatDataSet[i]);
            }
            let HeatSumData = [];
            // for (let i = 0; i < 4; ++i) {
            //     HeatSumData.push(lineData[i]);
            // }
            for (let i = 0; i < heatBeforeDataSet.length; ++i) {
                HeatSumData.push(heatBeforeDataSet[i]);
            }

            // for (let i = 0; i < heatDataSet.length; ++i) {
            //     HeatSumData.push(heatDataSet[i]);
            // }
            let rawScale = scaleLinear([minRaw, maxRaw], [0, 1]);
            let rmseScale = scaleLinear([minRmse, maxRmse], [0, 1]);
            let errorScale = scaleLinear([minError, maxError], [0, 1]);
            let corrScale = scaleLinear([minCorr, maxCorr], [0, 1]);
            let timeScale = scaleLinear([0, maxTime], [margin.left, width - margin.right]);
            let quantization1 = vsup.quantization().branching(2).layers(4).valueDomain([minError, maxError]).uncertaintyDomain([(maxRaw), minRaw]);
            let quantization2 = vsup.quantization().branching(2).layers(4).valueDomain([minRmse, maxRmse]).uncertaintyDomain([(maxCorr), minCorr]);

            let heatColor = interpolateYlOrRd;
            let heatScale1 = vsup.scale().quantize(quantization1).range(heatColor);
            let heatScale2 = vsup.scale().quantize(quantization2).range(heatColor);

            // var legend = vsup.legend.arcmapLegend();

            // legend
            //     .scale(heatScale)
            //     .size(160)
            //     .x(200)
            //     .y(100)
            //     .vtitle("Difference")
            //     .utitle("RMSE");
            // select('#legend_g').append('g')
            // .call(legend)
            // let xAxis = axisBottom(timeScale).ticks(10);
            // let yAxis = axisLeft(rmseScale).ticks(10);
            // select("#x_axis_g").call(xAxis);
            // select("#y_axis_g").call(yAxis);
            let res_data = [];
            for (let i in HeatSumData) {
                for (let j in HeatSumData[i]) {
                    // console.log(lineData[i][j].skip)
                    HeatSumData[i][j].x = timeScale(HeatSumData[i][j].time);
                    HeatSumData[i][j].w = timeScale(HeatSumData[i][j].skip);
                    HeatSumData[i][j].v = rmseScale(HeatSumData[i][j].rmse);
                    HeatSumData[i][j].id_cnt = HeatSumData[i][j].id_cnt;
                    HeatSumData[i][j].h = height / 36 - 3
                    HeatSumData[i][j].y = parseInt(HeatSumData[i][j].id) * height / 36;
                    HeatSumData[i][j].rmseColor = heatColor(HeatSumData[i][j].v);
                    HeatSumData[i][j].errorColor = heatColor(errorScale(HeatSumData[i][j].errorData));
                    HeatSumData[i][j].rawColor = heatColor(rawScale(HeatSumData[i][j].rawData));
                    HeatSumData[i][j].corrColor = heatColor(corrScale(HeatSumData[i][j].corr));
                    HeatSumData[i][j].vsupColor1 = heatScale1((HeatSumData[i][j].errorData), (HeatSumData[i][j].rawData));
                    HeatSumData[i][j].vsupColor2 = heatScale2((HeatSumData[i][j].rmse), HeatSumData[i][j].corr);
                    HeatSumData[i][j].colorMap = [HeatSumData[i][j].vsupColor1, HeatSumData[i][j].rawColor, HeatSumData[i][j].errorColor, HeatSumData[i][j].vsupColor2, HeatSumData[i][j].rmseColor, HeatSumData[i][j].corrColor];
                }
                res_data.push({
                    h: height / 36,
                    heat: HeatSumData[i]
                })
            }
            // console.log(res_data);

            return res_data;
        },
        calcRMSEHeatMultiVariable (data, raw_data, width, height) {
            let margin = ({ top: 20, right: 20, bottom: 30, left: 50 });
            // let sdata = [];
            let allStrip = data.length;
            let maxRmse = -999999;
            let minRmse = 999999;
            let maxTime = -999999;
            let maxError = -999999;
            let minError = 999999;
            let maxRaw = -999999;
            let minRaw = 999999;
            let maxCorr = -999999;
            let minCorr = 999999;
            let heatDataSet = [];
            let heatBeforeDataSet = [];
            let id_cnt = 0;
            // for (let kk in data) {
            //     let heat_data = [];
            //     let heatBefore_data = [];
            //     // let smooth_data = smooth_dataSet[kk];
            //     let skipLength = this.skip_length[parseInt(kk)];
            //     // console.log(smooth_data, skipLength);
            //     for (let i = 0; i < 840; i += (i == 0 && 840 % skipLength != 0 ? 840 % skipLength : skipLength)) {
            //         let skp = (i == 0 && 840 % skipLength != 0 ? 840 % skipLength : skipLength);
            //         let rawTempValue = raw_data.slice(i, i + skp).map(d => parseFloat(d.value)).sort((a, b) => a - b)
            //         let rawSum = sum(rawTempValue);
            //         // let smoothTempValue = smooth_data.slice(i, i + skp).map(d => parseFloat(d.value)).sort((a, b) => a - b)
            //         // let smoothSum = sum(smoothTempValue);
            //         heatBefore_data.push({
            //             // rawData: smoothSum / smoothTempValue.length,
            //             // errorData: Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length),
            //             time: i,
            //             rmse: 0,
            //             corr: 0,
            //             skip: skp,
            //             id: parseInt(kk),
            //             fill_opacity: 0,
            //             id_cnt: '_1'
            //             // x: x(parseInt(raw_data[i].id)),
            //             // w: Math.abs(x(parseInt(raw_data[i + ((i + skipLength < raw_data.length) ? skipLength : (raw_data.length - 1 - i))].id)) - x(parseInt(raw_data[i].id))),
            //         });
            //         // maxError = Math.max(maxError, Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length));
            //         // minError = Math.min(minError, Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length));
            //         // maxRaw = Math.max(maxRaw, smoothSum / smoothTempValue.length);
            //         // minRaw = Math.min(minRaw, smoothSum / smoothTempValue.length);
            //     }
            //     heatBeforeDataSet.push(heatBefore_data);
            //     for (let i = 840; i < raw_data.length; i += skipLength) {
            //         // let rawTempValue = raw_data.slice(i, i + skipLength).map(d => parseFloat(d.value)).sort((a, b) => a - b)
            //         // let rawSum = sum(rawTempValue);
            //         // let smoothTempValue = smooth_data.slice(i, i + skipLength).map(d => parseFloat(d.value)).sort((a, b) => a - b)
            //         // let smoothSum = sum(smoothTempValue);
            //         heat_data.push({
            //             // rawData: smoothSum / smoothTempValue.length,
            //             // errorData: Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length),
            //             time: i,
            //             rmse: 0,
            //             corr: 0,
            //             skip: skipLength,
            //             id: parseInt(kk),
            //             fill_opacity: 1,
            //             id_cnt: id_cnt++
            //             // x: x(parseInt(raw_data[i].id)),
            //             // w: Math.abs(x(parseInt(raw_data[i + ((i + skipLength < raw_data.length) ? skipLength : (raw_data.length - 1 - i))].id)) - x(parseInt(raw_data[i].id))),
            //         });
            //         // maxError = Math.max(maxError, Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length));
            //         // minError = Math.min(minError, Math.abs(rawSum / rawTempValue.length - smoothSum / smoothTempValue.length));
            //         // maxRaw = Math.max(maxRaw, smoothSum / smoothTempValue.length);
            //         // minRaw = Math.min(minRaw, smoothSum / smoothTempValue.length);
            //     }
            //     heatDataSet.push(heat_data);
            // }
            // console.log(heatBeforeDataSet);

            let lineData = [];
            let cnt_id_cnt = 0;
            for (let i in data) {
                let startPos = 0;
                let tp = [];
                for (let j in data[i]) {
                    if (j > data[i].length / 2) break;
                    // sdata.push({
                    //     id: i,
                    //     skip: this.skip_length[i],
                    //     time: j * this.skip_length[i] + startPos,
                    //     rmse: parseFloat(data[i][j]['rmse'])
                    // });
                    tp.push({
                        id: i,
                        skip: parseInt(data[i][j]['skip']),
                        time: j * parseInt(data[i][j]['skip']) + startPos,
                        errorData: 0,
                        id_cnt: cnt_id_cnt++,
                        rmse: parseFloat(data[i][j]['rmse']),
                        corr: parseFloat(data[i][j]['result_corr'])
                    });
                    // console.log(data[i][j]['129_pearson'])
                    maxTime = Math.max(maxTime, j * parseInt(data[i][j]['skip']) + startPos);
                    maxRmse = Math.max(maxRmse, parseFloat(data[i][j]['rmse']));
                    minRmse = Math.min(minRmse, parseFloat(data[i][j]['rmse']));
                    maxCorr = Math.max(maxCorr, parseFloat(data[i][j]['result_corr']));
                    if (parseFloat(data[i][j]['result_corr']) != 0)
                        minCorr = Math.min(minCorr, parseFloat(data[i][j]['result_corr']));
                }
                lineData.push(tp);
            }
            // console.log(heatDataSet)
            // console.log(lineData);
            // console.log(maxCorr, minCorr);


            // for (let i in heatDataSet) {
            //     for (let j in heatDataSet[i]) {
            //         heatDataSet[i][j]['rmse'] = lineData[parseInt(i)][j]['rmse'];
            //         heatDataSet[i][j]['corr'] = lineData[parseInt(i)][j]['corr'];
            //         heatBeforeDataSet[i].push(heatDataSet[i][j])
            //     }
            //     heatBeforeDataSet[i] = heatBeforeDataSet[i].concat(heatDataSet[i]);
            // }
            let HeatSumData = [];
            // for (let i = 0; i < 4; ++i) {
            //     HeatSumData.push(lineData[i]);
            // }
            for (let i = 0; i < lineData.length; ++i) {
                HeatSumData.push(lineData[i]);
            }

            // for (let i = 0; i < heatDataSet.length; ++i) {
            //     HeatSumData.push(heatDataSet[i]);
            // }
            // let rawScale = scaleLinear([minRaw, maxRaw], [0, 1]);
            let rmseScale = scaleLinear([minRmse, maxRmse], [0, 1]);
            // let errorScale = scaleLinear([minError, maxError], [0, 1]);
            let corrScale = scaleLinear([minCorr, maxCorr], [0, 1]);
            let timeScale = scaleLinear([0, maxTime], [margin.left, width - margin.right]);
            // let quantization1 = vsup.quantization().branching(2).layers(4).valueDomain([minError, maxError]).uncertaintyDomain([(maxRaw), minRaw]);
            let quantization2 = vsup.quantization().branching(2).layers(4).valueDomain([minRmse, maxRmse]).uncertaintyDomain([(maxCorr), minCorr]);

            let heatColor = interpolateYlOrRd;
            // let heatScale1 = vsup.scale().quantize(quantization1).range(heatColor);
            let heatScale2 = vsup.scale().quantize(quantization2).range(heatColor);

            // var legend = vsup.legend.arcmapLegend();

            // legend
            //     .scale(heatScale)
            //     .size(160)
            //     .x(200)
            //     .y(100)
            //     .vtitle("Difference")
            //     .utitle("RMSE");
            // select('#legend_g').append('g')
            // .call(legend)
            // let xAxis = axisBottom(timeScale).ticks(10);
            // let yAxis = axisLeft(rmseScale).ticks(10);
            // select("#x_axis_g").call(xAxis);
            // select("#y_axis_g").call(yAxis);
            let res_data = [];
            for (let i in HeatSumData) {
                for (let j in HeatSumData[i]) {
                    // console.log(lineData[i][j].skip)
                    HeatSumData[i][j].x = timeScale(HeatSumData[i][j].time);
                    HeatSumData[i][j].w = timeScale(HeatSumData[i][j].skip);
                    HeatSumData[i][j].v = rmseScale(HeatSumData[i][j].rmse);
                    HeatSumData[i][j].id_cnt = HeatSumData[i][j].id_cnt;
                    HeatSumData[i][j].h = height / allStrip - 3
                    HeatSumData[i][j].y = parseInt(HeatSumData[i][j].id) * height / allStrip;
                    HeatSumData[i][j].rmseColor = heatColor(HeatSumData[i][j].v);
                    // HeatSumData[i][j].errorColor = heatColor(errorScale(HeatSumData[i][j].errorData));
                    // HeatSumData[i][j].rawColor = heatColor(rawScale(HeatSumData[i][j].rawData));
                    HeatSumData[i][j].corrColor = heatColor(corrScale(HeatSumData[i][j].corr));
                    // HeatSumData[i][j].vsupColor1 = heatScale1((HeatSumData[i][j].errorData), (HeatSumData[i][j].rawData));
                    HeatSumData[i][j].vsupColor2 = heatScale2((HeatSumData[i][j].rmse), HeatSumData[i][j].corr);
                    HeatSumData[i][j].colorMap = ['grey', 'grey', 'grey', HeatSumData[i][j].vsupColor2, HeatSumData[i][j].rmseColor, HeatSumData[i][j].corrColor];
                }
                res_data.push({
                    h: height / allStrip,
                    heat: HeatSumData[i]
                })
            }
            // console.log(res_data);

            return res_data;
        }
    },
    created () {
    },
    mounted () {
        this.elHeight = this.$refs.DataTransformation.offsetHeight;
        this.elWidth = this.$refs.DataTransformation.offsetWidth;

        const importData = import.meta.globEager('../assets/multivarData/*.csv');
        // console.log(importData)
        let multiDataSet = []
        for (let i in importData) {
            multiDataSet.push(importData[i]['default'])
            console.log(importData[i], i)
        }
        let dataSet = [d0, d1, d2, d3, d4, d5, d6, d7, d8, d9, d10, d11, d12, d13, d14, d15, d16, d17, d18, d19, d20, d21, d22, d23, d24, d25, d26, d27, d28, d29, d30, d31, d32, d33, d34, d35];
        let smoothDataSet = [SN_raw_data, SN_raw_data, SN_raw_data, SN_raw_data, sr13, sr13, sr13, sr13, sr3, sr3, sr3, sr3, sr6, sr6, sr6, sr6, sr9, sr9, sr9, sr9, sa13, sa13, sa13, sa13, sa3, sa3, sa3, sa3, sa6, sa6, sa6, sa6, sa9, sa9, sa9, sa9];

        this.heatRectData = this.calcRMSEHeatMultiVariable(multiDataSet, SN_raw_data, this.elWidth, this.elHeight - 20);

        // this.heatRectData = this.calcRMSEHeat(dataSet, smoothDataSet, SN_raw_data, this.elWidth, this.elHeight - 20);


        const dataStore = useDataStore();
        let _this = this;

        dataStore.$subscribe((mutation, state) => {
            // console.log(dataStore.selectDot, _this.heatRectData);
            selectAll('#tsr_1').attr('opacity', 0);
            let coverRect = [];
            for (let i in _this.heatRectData) {
                for (let j in _this.heatRectData[i].heat) {
                    // console.log(_this.heatRectData[i][j])

                    if (dataStore.selectDot[this.heatRectData[i].heat[j].id_cnt] == 1) {
                        // select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('opacity', 1).attr('stroke', 'blue').attr('stroke-width', 0);
                        coverRect.push({
                            x: select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('x'),
                            y: select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('y'),
                            w: select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('width'),
                            h: select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('height'),
                            fill: select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('fill'),
                            cnt: select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('class'),
                        })
                    }
                    // else select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('opacity', 0)
                    // .attr('fill', '#d9d9d9');
                }
            }
            console.log(coverRect);
            this.coverRect = coverRect;
        })

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

.selection {
    fill-opacity: 0.15;
}
</style>
