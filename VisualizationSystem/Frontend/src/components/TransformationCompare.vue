<!--
 * @Author: Qing Shi
 * @LastEditTime: 2023-03-26 19:18:38
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
            <span style="margin-right: 20px; margin-top: 0px;">
                            <el-button style="height: 30px; width: 30px;" @click="refresh()">
                                <svg t="1679773906391" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2762" width="20" height="20"><path d="M512 0C229.230208 0 0 229.230208 0 512 0 794.769792 229.230208 1024 512 1024 761.325865 1024 973.201958 844.559514 1016.153097 601.764678 1018.151127 590.470182 1019.771663 579.089182 1021.010022 567.635639 1022.492753 553.921916 1012.577574 541.602754 998.863851 540.120021 985.150125 538.637291 972.830963 548.552469 971.348233 562.266193 970.230573 572.603369 968.768273 582.873092 966.965602 593.063262 928.217702 812.097967 736.992706 974.048781 512 974.048781 256.817504 974.048781 49.951219 767.182496 49.951219 512 49.951219 256.817504 256.817504 49.951219 512 49.951219 698.044361 49.951219 863.703281 160.916567 936.293348 328.7543 941.768939 341.414579 956.470965 347.238921 969.131243 341.763332 981.791522 336.287742 987.615863 321.585717 982.140273 308.925438 901.710383 122.961007 718.143277 0 512 0Z" fill="#979797" p-id="2763"></path><path d="M958.442797 350.246554 983.418406 325.270944 724.292683 325.270944C710.499034 325.270944 699.317073 336.452907 699.317073 350.246554 699.317073 364.040203 710.499034 375.222163 724.292683 375.222163L983.418406 375.222163C997.212055 375.222163 1008.394016 364.040203 1008.394016 350.246554L1008.394016 74.926829C1008.394016 61.133181 997.212055 49.951219 983.418406 49.951219 969.624757 49.951219 958.442797 61.133181 958.442797 74.926829L958.442797 350.246554Z" fill="#979797" p-id="2764"></path></svg>
                            </el-button>
                        </span>
            <span>Metric: </span>
            <el-select v-model="heatTag" class="m-2" placeholder="Select" style="width: 150px;">
                <el-option v-for="(item, i) in heatOptions" :key="item" :label="item" :value="i" />
            </el-select>
        </div>
    </div>
    <div class="frameworkBody">
        <div ref="DataTransformation" style="height: calc(97%); width: 100%; margin-top: 0px;">
            <svg height="100%" width="100%" transform="translate(0, 0)">
                                    <g v-for="(item, i) in heatRectData" :key="'heat_g' + i" :transform="translate(0, item.h * i, 0)"
                                        @mouseenter="selectFile(i)" @mouseout="cancelFile(i)" @click="clickFile(i, item.class_name)">
                                        <rect v-for="(item_h, item_i) in item['heat']" :key="'heat_' + item_i" :x="item_h.x" :y="0"
                                            :width="item_h.w" :height="item_h.h" :fill="item_h.colorMap[heatTag]" :id="'tsr' + item_h.id_cnt"
                                            :class="'wsr' + i" :fill-opacity="1">
                                        </rect>
                                        <rect v-for="(item_h, item_i) in item['res']" :key="'heat_' + item_i" :x="item_h.x" :y="0"
                                            :width="item_h.w" :height="item_h.h" :fill="'orange'" :id="'tsr' + item_i"
                                            :fill-opacity="item_h.fill_opacity">
                                        </rect>
                    
                                        <text font-size="14" text-anchor="start" dx="0em" dy="1em">{{
                                            // filename[i].substring(0, filename[i].length - 8)
                                            filename[dataSelect][i].smooth + '/Skip-' + filename[dataSelect][i].skip
                                        }}</text>
                                    </g>
                                    <g id="legend_g"></g>
                                    <!-- <g v-for="(item, i) in heatRectData" :key="'heat_g' + i" :transform="translate(0, item.h * i, 0)">
                                        <rect :id="'rst' + i" class="rst" :x="item['heat'][0].x" :y="0" :width="elWidth - 10 - item['heat'][0].x"
                                            :height="item['heat'][0].h" :fill="checkSelectStatus(i) == 2 ? '#c0c0c0' : 'none'" stroke="black" :stroke-width="checkSelectStatus(i) == 1 ? 3 : 0" :fill-opacity="checkSelectStatus(i) == 2 ? 0 : 0"></rect>
                                    </g> -->
                                    <g v-for="(item, i) in coverRect" :key="'heat_g' + i"
                                        :transform="translate(0, item.realH, 0)">
                                        <rect :id="'rst' + i" class="rst" :x="item.x" :y="0" :width="item.w" :height="item.h" :fill="item.colorMap[heatTag]"
                                            stroke="none" stroke-width="0"></rect>
                                    </g>
                                    <!-- <g v-for="(item, i) in groupPath" :key="'group_g' + i" :transform="translate(0, 0, 0)">
                                        <path :d="'M ' + item.x1 + ' ' + item.y1 + ' L ' + item.x2 + ' ' + item.y2" fill="none" stroke="black">
                                        </path>
                                </g> -->
                    
                            </svg>
    
        </div>
        <div ref="RepresentationTimeAxis" style="height: 3%; width: 100%;">
            <svg width="100%" height="100%">
                        <g id="representationTime"></g>
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
import { select, selectAll } from 'd3-selection';
import { extent, max, min, sum } from 'd3-array';
import { brushX } from 'd3-brush';
import * as vsup from 'vsup';


// univariate
import d0 from '../assets/allData/univariate_data/result_data/rawdata_skip13_0.8.csv';
import d1 from '../assets/allData/univariate_data/result_data/rawdata_skip6_0.8.csv';
import d2 from '../assets/allData/univariate_data/result_data/rawdata_skip3_0.8.csv';
import d3 from '../assets/allData/univariate_data/result_data/rawdata_skip1_0.8.csv';
import d4 from '../assets/allData/univariate_data/result_data/rolling3_skip13_0.8.csv';
import d5 from '../assets/allData/univariate_data/result_data/rolling3_skip6_0.8.csv';
import d6 from '../assets/allData/univariate_data/result_data/rolling3_skip3_0.8.csv';
import d7 from '../assets/allData/univariate_data/result_data/rolling3_skip1_0.8.csv';
import d8 from '../assets/allData/univariate_data/result_data/weighted3_skip13_0.8.csv';
import d9 from '../assets/allData/univariate_data/result_data/weighted3_skip6_0.8.csv';
import d10 from '../assets/allData/univariate_data/result_data/weighted3_skip3_0.8.csv';
import d11 from '../assets/allData/univariate_data/result_data/weighted3_skip1_0.8.csv';
import d12 from '../assets/allData/univariate_data/result_data/rolling6_skip13_0.8.csv';
import d13 from '../assets/allData/univariate_data/result_data/rolling6_skip6_0.8.csv';
import d14 from '../assets/allData/univariate_data/result_data/rolling6_skip3_0.8.csv';
import d15 from '../assets/allData/univariate_data/result_data/rolling6_skip1_0.8.csv';
import d16 from '../assets/allData/univariate_data/result_data/weighted6_skip13_0.8.csv';
import d17 from '../assets/allData/univariate_data/result_data/weighted6_skip6_0.8.csv';
import d18 from '../assets/allData/univariate_data/result_data/weighted6_skip3_0.8.csv';
import d19 from '../assets/allData/univariate_data/result_data/weighted6_skip1_0.8.csv';
import d20 from '../assets/allData/univariate_data/result_data/rolling9_skip13_0.8.csv';
import d21 from '../assets/allData/univariate_data/result_data/rolling9_skip6_0.8.csv';
import d22 from '../assets/allData/univariate_data/result_data/rolling9_skip3_0.8.csv';
import d23 from '../assets/allData/univariate_data/result_data/rolling9_skip1_0.8.csv';
import d24 from '../assets/allData/univariate_data/result_data/weighted9_skip13_0.8.csv';
import d25 from '../assets/allData/univariate_data/result_data/weighted9_skip6_0.8.csv';
import d26 from '../assets/allData/univariate_data/result_data/weighted9_skip3_0.8.csv';
import d27 from '../assets/allData/univariate_data/result_data/weighted9_skip1_0.8.csv';
import d28 from '../assets/allData/univariate_data/result_data/rolling13_skip13_0.8.csv';
import d29 from '../assets/allData/univariate_data/result_data/rolling13_skip6_0.8.csv';
import d30 from '../assets/allData/univariate_data/result_data/rolling13_skip3_0.8.csv';
import d31 from '../assets/allData/univariate_data/result_data/rolling13_skip1_0.8.csv';
import d32 from '../assets/allData/univariate_data/result_data/weighted13_skip13_0.8.csv';
import d33 from '../assets/allData/univariate_data/result_data/weighted13_skip6_0.8.csv';
import d34 from '../assets/allData/univariate_data/result_data/weighted13_skip3_0.8.csv';
import d35 from '../assets/allData/univariate_data/result_data/weighted13_skip1_0.8.csv';

// multivariate smooth data
import ms0 from '../assets/allData/multivariate_data/smooth_data/raw_15month.csv';
import ms1 from '../assets/allData/multivariate_data/smooth_data/rolling12_15month.csv';
import ms2 from '../assets/allData/multivariate_data/smooth_data/rolling3_15month.csv';
import ms3 from '../assets/allData/multivariate_data/smooth_data/rolling6_15month.csv';
import ms4 from '../assets/allData/multivariate_data/smooth_data/weighted12_15month.csv';
import ms5 from '../assets/allData/multivariate_data/smooth_data/weighted3_15month.csv';
import ms6 from '../assets/allData/multivariate_data/smooth_data/weighted6_15month.csv';

// multivariate
import m0 from '../assets/allData/multivariate_data/result_data/raw_skip2.csv';
import m1 from '../assets/allData/multivariate_data/result_data/raw_skip3.csv';
import m2 from '../assets/allData/multivariate_data/result_data/weighted6_skip1.csv';
import m3 from '../assets/allData/multivariate_data/result_data/weighted6_skip3.csv';
import m4 from '../assets/allData/multivariate_data/result_data/raw_skip1.csv';
import m5 from '../assets/allData/multivariate_data/result_data/rolling6_skip6.csv';
import m6 from '../assets/allData/multivariate_data/result_data/weighted6_skip2.csv';
import m7 from '../assets/allData/multivariate_data/result_data/weighted6_skip6.csv';
import m8 from '../assets/allData/multivariate_data/result_data/rolling6_skip2.csv';
import m9 from '../assets/allData/multivariate_data/result_data/rolling6_skip3.csv';
import m10 from '../assets/allData/multivariate_data/result_data/rolling6_skip1.csv';
import m11 from '../assets/allData/multivariate_data/result_data/raw_skip6.csv';
import m12 from '../assets/allData/multivariate_data/result_data/rolling12_skip1.csv';
import m13 from '../assets/allData/multivariate_data/result_data/rolling12_skip2.csv';
import m14 from '../assets/allData/multivariate_data/result_data/rolling12_skip3.csv';
import m15 from '../assets/allData/multivariate_data/result_data/rolling12_skip6.csv';
import m16 from '../assets/allData/multivariate_data/result_data/rolling3_skip2.csv';
import m17 from '../assets/allData/multivariate_data/result_data/weighted3_skip6.csv';
import m18 from '../assets/allData/multivariate_data/result_data/weighted12_skip3.csv';
import m19 from '../assets/allData/multivariate_data/result_data/weighted12_skip2.csv';
import m20 from '../assets/allData/multivariate_data/result_data/rolling3_skip3.csv';
import m21 from '../assets/allData/multivariate_data/result_data/rolling3_skip1.csv';
import m22 from '../assets/allData/multivariate_data/result_data/weighted12_skip1.csv';
import m23 from '../assets/allData/multivariate_data/result_data/weighted3_skip1.csv';
import m24 from '../assets/allData/multivariate_data/result_data/weighted3_skip3.csv';
import m25 from '../assets/allData/multivariate_data/result_data/weighted12_skip6.csv';
import m26 from '../assets/allData/multivariate_data/result_data/weighted3_skip2.csv';
import m27 from '../assets/allData/multivariate_data/result_data/rolling3_skip6.csv';

import rs1 from '../assets/allData/multivariate_data/new_res_data/rawdata_skip1.csv';
import rs2 from '../assets/allData/multivariate_data/new_res_data/rawdata_skip6.csv'
import rs3 from '../assets/allData/multivariate_data/new_res_data/rawdata_skip8.csv'
import rs4 from '../assets/allData/multivariate_data/new_res_data/rawdata_skip12.csv'
import rs5 from '../assets/allData/multivariate_data/new_res_data/rawdata_skip24.csv'

export default {
    name: 'DataTransformationView',
    props: ['timeData', 'sliceData'],
    data() {
        return {
            elHeight: 1000,
            elWidth: 1000,
            tlHeight: 100,
            tlWidth: 100,
            heatHeight: 0,
            heatTag: 4,
            clickFileTag: 0,
            heatOptions: ['Raw + Difference', 'Raw', 'Difference', 'RMSE + Corr.', 'RMSE', 'Corr.'],
            sample: ['10-slice', '7-slice', '3-slice'],
            smooth: ['Raw Sequence', 'N-Average', 'EMA/Holt'],
            sparkboxData: [],
            heatRectData: [],
            lineData: [],
            smoothLineData: [],
            skiplength: [13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6],
            columnData: ['Smooth', 'Skip', 'Train-Loss', 'Test-Loss', 'ACF', 'Window Performance'],
            filename: {'sunspots': [{ smooth: 'RAW', skip: '13' },
                { smooth: 'RAW', skip: '6' },
                { smooth: 'RAW', skip: '3' },
                { smooth: 'RAW', skip: '1' },
                { smooth: 'MA-3', skip: '13' },
                { smooth: 'MA-3', skip: '6' },
                { smooth: 'MA-3', skip: '3' },
                { smooth: 'MA-3', skip: '1' },
                { smooth: 'WMA-3', skip: '13' },
                { smooth: 'WMA-3', skip: '6' },
                { smooth: 'WMA-3', skip: '3' },
                { smooth: 'WMA-3', skip: '1' },
                { smooth: 'MA-6', skip: '13' },
                { smooth: 'MA-6', skip: '6' },
                { smooth: 'MA-6', skip: '3' },
                { smooth: 'MA-6', skip: '1' },
                { smooth: 'WMA-6', skip: '13' },
                { smooth: 'WMA-6', skip: '6' },
                { smooth: 'WMA-6', skip: '3' },
                { smooth: 'WMA-6', skip: '1' },
                { smooth: 'MA-9', skip: '13' },
                { smooth: 'MA-9', skip: '6' },
                { smooth: 'MA-9', skip: '3' },
                { smooth: 'MA-9', skip: '1' },
                { smooth: 'WMA-9', skip: '13' },
                { smooth: 'WMA-9', skip: '6' },
                { smooth: 'WMA-9', skip: '3' },
                { smooth: 'WMA-9', skip: '1' },
                { smooth: 'MA-13', skip: '13' },
                { smooth: 'MA-13', skip: '6' },
                { smooth: 'MA-13', skip: '3' },
                { smooth: 'MA-13', skip: '1' },
                { smooth: 'WMA-13', skip: '13' },
                { smooth: 'WMA-13', skip: '6' },
                { smooth: 'WMA-13', skip: '3' },
                { smooth: 'WMA-13', skip: '1' }
            ], 'pm': [{ smooth: 'RAW', skip: '2' },
                { smooth: 'RAW', skip: '3' },
                { smooth: 'WMA-6', skip: '1' },
                { smooth: 'WMA-6', skip: '3' },
                { smooth: 'RAW', skip: '1' },
                { smooth: 'MA-6', skip: '6' },
                { smooth: 'WMA-6', skip: '2' },
                { smooth: 'WMA-6', skip: '6' },
                { smooth: 'MA-6', skip: '2' },
                { smooth: 'MA-6', skip: '3' },
                { smooth: 'MA-6', skip: '1' },
                { smooth: 'RAW', skip: '6' },
                { smooth: 'MA-12', skip: '1' },
                { smooth: 'MA-12', skip: '2' },
                { smooth: 'MA-12', skip: '3' },
                { smooth: 'MA-12', skip: '6' },
                { smooth: 'MA-3', skip: '2' },
                { smooth: 'WMA-3', skip: '6' },
                { smooth: 'WMA-12', skip: '3' },
                { smooth: 'WMA-12', skip: '2' },
                { smooth: 'MA-3', skip: '3' },
                { smooth: 'MA-3', skip: '1' },
                { smooth: 'WMA-12', skip: '1' },
                { smooth: 'WMA-3', skip: '1' },
                { smooth: 'WMA-3', skip: '1' },
                { smooth: 'WMA-12', skip: '6' },
                { smooth: 'WMA-3', skip: '2' },
                { smooth: 'MA-3', skip: '6' }
            ]},
            coverRect: [],
            dataSelect: 'sunspots',
            allTimeScale: {
                'sunspots': {
                    start: '1878-01-01',
                    end: '2013-01-01'
                },
                'pm': {
                    start: '2013-01-02 00:00:00',
                    end: '2014-03-31 12:00:00'
                }
            },
            selectRepresentationRow: {
                'sunspots': {
                    tag: 0,
                    status: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
                },
                'pm': {
                    tag: 0,
                    status: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
                }
            },
            selectDot: {
                tag: 0,
                data: []
            }
        }
    },
    methods: {
        refresh() {
            this.selectRepresentationRow[this.dataSelect].tag = 0;
            this.coverRect = [];
            const dataStore = useDataStore();
            dataStore.selectRepresentation.data = {};
            dataStore.selectRepresentation.tag = !dataStore.selectRepresentation.tag;

            selectAll('.corr_cir_out').remove();
            selectAll('.corr_cir').attr('opacity', (d, i) => {
                return d.isShow ? 0 : 1;
            }).attr('fill', (d, i) => {
                // if (d.class_name == this.filename[num].substring(0, this.filename[num].length - 8)) return 'orange';
                // else 
                return d.fill;
            })
        },
        checkSelectStatus: function(num) {
            if (this.selectRepresentationRow[this.dataSelect].tag == 0)
            return 0;
            if (this.selectRepresentationRow[this.dataSelect].status[num]) {
                return 1;
            } else {
                return 2;
            }
        },
        selectFile(num) {
            if (this.clickFileTag) return;
            select('#rst' + num).attr('stroke-width', 3)
            // .attr('fill', '#777').attr('fill-opacity', 0.5);
            selectAll('.p_x').attr('opacity', (d, i) => {
                return d.id == num ? 1 : 0;
            })
        },
        clickFile(num, class_name) {
            let tdata = []
            this.clickFileTag = 1;
            // console.log(class_name );
            this.selectRepresentationRow[this.dataSelect].status[num] = 1;
            this.selectRepresentationRow[this.dataSelect].tag = 1;
            selectAll('.corr_cir').attr('opacity', (d, i) => {

                if (d.class_name == class_name) {
                    tdata.push(d);
                    // return 0.5;
                }
                return d.isShow ? 0 : 0.5;
            }).attr('fill', (d, i) => {
                // if (d.class_name == this.filename[num].substring(0, this.filename[num].length - 8)) return 'orange';
                // else 
                return '#d9d9d9';
            })
            // console.log(tdata)
            let select_dot = {};
            // console.log(this.heatRectData[num]);
            for (let i in this.heatRectData[num].heat) {
                select_dot[this.heatRectData[num].heat[i]['uid']] = 1;
            }
            // console.log(select_dot);
            const dataStore = useDataStore();
            dataStore.selectRepresentation.data = select_dot;
            dataStore.selectRepresentation.tag = !dataStore.selectRepresentation.tag;
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
        cancelFile(num) {
            if (this.clickFileTag) return;
            select('#rst' + num).attr('stroke-width', 0);
            selectAll('.p_x').attr('opacity', 1)
            // selectAll('.corr_cir').attr('opacity', (d, i) => {
            //     // if (d.class_name == this.filename[num].substring(0, this.filename[num].length - 8)) {
            //     //     return 1;
            //     // }
            //     return 1;
            // })
        },
        translate(x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        },
        setupBrush: function(data) {
            let width = this.tlWidth;
            let height = this.tlHeight;
            let focusHeight = 100;
            let margin = ({ top: 20, right: 20, bottom: 30, left: 50 });
            const timeBrush = brushX()
                .extent([
                    [margin.left, margin.top],
                    [width - margin.right, focusHeight]
                ])
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

            function brushStart() {
                selectAll('.sparkbox').remove();
            }

            function brushEnd({ selection }) {
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

            function brushed({ selection }) {
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
        calcRMSEHeat(data, smooth_dataSet, raw_data, width, height) {
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
                let skipLength = this.skiplength[parseInt(kk)];
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
                    //     skip: this.skiplength[i],
                    //     time: j * this.skiplength[i] + startPos,
                    //     rmse: parseFloat(data[i][j]['rmse'])
                    // });
                    tp.push({
                        id: i,
                        skip: this.skiplength[i],
                        time: j * this.skiplength[i] + startPos,
                        errorData: 0,
                        rmse: parseFloat(data[i][j]['rmse']),
                        corr: parseFloat(data[i][j]['result_corr'])
                    });
                    // console.log(data[i][j]['129_pearson'])
                    maxTime = Math.max(maxTime, j * this.skiplength[i] + startPos);
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
        calcRMSEHeatMultiVariable(data, width, height) {
            let margin = ({ top: 20, right: 10, bottom: 30, left: 50 });
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

            let lineData = [];
            let cnt_id_cnt = 0;
            for (let i in data) {
                // console.log(data[i])
                let startPos = 0;
                let tp = [];
                for (let j in data[i]) {
                    // if (j > data[i].length / 2.3) break;
                    // sdata.push({
                    //     id: i,
                    //     skip: this.skiplength[i],
                    //     time: j * this.skiplength[i] + startPos,
                    //     rmse: parseFloat(data[i][j]['rmse'])
                    // });
                    tp.push({
                        id: i,
                        skip: parseInt(data[i][j]['skip']),
                        time: j * parseInt(data[i][j]['skip']) + startPos,
                        errorData: 0,
                        id_cnt: cnt_id_cnt++,
                        uid: data[i][j]['smooth'] + '_' + data[i][j]['skip'] + '_' + j,
                        cid: data[i][j]['smooth'] + '_' + data[i][j]['skip'],
                        rmse: parseFloat(data[i][j]['rmse']),
                        corr: parseFloat(data[i][j]['result_corr'])
                    });
                    // console.log(data[i][j]['129_pearson'])
                    maxTime = Math.max(maxTime, (parseInt(j) + 1) * parseInt(data[i][j]['skip']) + startPos);
                    maxRmse = Math.max(maxRmse, parseFloat(data[i][j]['rmse']));
                    minRmse = Math.min(minRmse, parseFloat(data[i][j]['rmse']));
                    maxCorr = Math.max(maxCorr, parseFloat(data[i][j]['result_corr']));
                    if (parseFloat(data[i][j]['result_corr']) != 0)
                        minCorr = Math.min(minCorr, parseFloat(data[i][j]['result_corr']));
                }
                lineData.push(tp);
            }
            let HeatSumData = [];
            for (let i = 0; i < lineData.length; ++i) {
                HeatSumData.push(lineData[i]);
            }

            let rmseScale = scaleLinear([minRmse, maxRmse], [0, 1]);
            let corrScale = scaleLinear([minCorr, maxCorr], [0, 1]);
            let timeScale = scaleLinear([0, maxTime], [margin.left, width - margin.right]);
            let quantization2 = vsup.quantization().branching(2).layers(4).valueDomain([minRmse, maxRmse]).uncertaintyDomain([(maxCorr), minCorr]);

            let heatColor = interpolateYlOrRd;
            let heatScale2 = vsup.scale().quantize(quantization2).range(heatColor);

            let res_data = [];

            for (let i in HeatSumData) {
                for (let j in HeatSumData[i]) {
                    HeatSumData[i][j].x = timeScale(HeatSumData[i][j].time);
                    HeatSumData[i][j].w = timeScale(HeatSumData[i][j].skip) - timeScale(0);
                    HeatSumData[i][j].v = rmseScale(HeatSumData[i][j].rmse);
                    HeatSumData[i][j].id_cnt = HeatSumData[i][j].id_cnt;
                    HeatSumData[i][j].h = height / allStrip - 3;
                    HeatSumData[i][j].allH = height / allStrip;
                    HeatSumData[i][j].realH = height / allStrip * i;
                    HeatSumData[i][j].y = parseInt(HeatSumData[i][j].id) * height / allStrip;
                    HeatSumData[i][j].rmseColor = heatColor(HeatSumData[i][j].v);
                    HeatSumData[i][j].corrColor = heatColor(corrScale(HeatSumData[i][j].corr));
                    HeatSumData[i][j].vsupColor2 = heatScale2((HeatSumData[i][j].rmse), HeatSumData[i][j].corr);
                    HeatSumData[i][j].colorMap = ['grey', 'grey', 'grey', HeatSumData[i][j].vsupColor2, HeatSumData[i][j].rmseColor, HeatSumData[i][j].corrColor];
                }
                // HeatSumData[i].sort((a, b) => a.v - b.v)
                res_data.push({
                    class_name: HeatSumData[i][0].cid,
                    h: height / allStrip,
                    heat: HeatSumData[i]
                })

            }
            // console.log(res_data);

            return res_data;
        },
        calcRMSETempHeatMultiVariate(data, smooth_data, width, height) {
            let margin = ({ top: 20, right: 20, bottom: 30, left: 50 });
            // let sdata = [];
            let allStrip = data.length;
            let maxRmse = -999999;
            let minRmse = 999999;
            let maxTime = -999999;
            let maxTemp = -999999;
            let minTemp = 999999;
            let maxCorr = -999999;
            let minCorr = 999999;
            let heatDataSet = [];
            let heatBeforeDataSet = [];
            let id_cnt = 0;

            let lineData = [];
            // console.log(smooth_data)
            let cnt_id_cnt = 0;
            for (let i in data) {
                let startPos = 0;
                let tp = [];
                for (let j in data[i]) {
                    let startTemp = j * parseInt(data[i][j]['skip']) + startPos;
                    let aveTemp = sum(smooth_data[i].slice(startTemp + 24, startTemp + 24 + 12), d => parseFloat(d.temp)) / 12;
                    // console.log(startTemp, smooth_data[i], smooth_data[i].slice(startTemp + 24, startTemp + 24 +  12))
                    // break

                    tp.push({
                        id: i,
                        skip: parseInt(data[i][j]['skip']),
                        time: j * parseInt(data[i][j]['skip']) + startPos,
                        errorData: 0,
                        id_cnt: cnt_id_cnt++,
                        aveTemp: aveTemp,
                        rmse: parseFloat(data[i][j]['rmse']),
                        corr: parseFloat(data[i][j]['result_corr'])
                    });
                    // console.log(data[i][j]['129_pearson'])
                    maxTime = Math.max(maxTime, (j) * parseInt(data[i][j]['skip']) + startPos);
                    maxRmse = Math.max(maxRmse, parseFloat(data[i][j]['rmse']));
                    minRmse = Math.min(minRmse, parseFloat(data[i][j]['rmse']));
                    maxCorr = Math.max(maxCorr, parseFloat(data[i][j]['result_corr']));
                    // if (parseFloat(data[i][j]['result_corr']) != 0)
                    minCorr = Math.min(minCorr, parseFloat(data[i][j]['result_corr']));
                    minTemp = Math.min(minTemp, aveTemp);
                    maxTemp = Math.max(maxTemp, aveTemp);
                }
                lineData.push(tp);
            }
            let HeatSumData = [];
            for (let i = 0; i < lineData.length; ++i) {
                HeatSumData.push(lineData[i]);
            }

            let rmseScale = scaleLinear([minRmse, maxRmse], [0, 1]);
            // let errorScale = scaleLinear([minError, maxError], [0, 1]);
            let tempScale = scaleLinear([minTemp, maxTemp], [0, 1]);
            let corrScale = scaleLinear([minCorr, maxCorr], [0, 1]);
            let timeScale = scaleLinear([0, maxTime], [margin.left, width - margin.right]);
            let quantization1 = vsup.quantization().branching(2).layers(4).valueDomain([165, 188]).uncertaintyDomain([(300), 295]);
            let quantization2 = vsup.quantization().branching(2).layers(4).valueDomain([minRmse, maxRmse]).uncertaintyDomain([(maxCorr), minCorr]);

            let heatColor = interpolateYlOrRd;
            let heatScale1 = vsup.scale().quantize(quantization1).range(heatColor);
            let heatScale2 = vsup.scale().quantize(quantization2).range(heatColor);

            var legend = vsup.legend.arcmapLegend();

            legend
                .scale(heatScale1)
                .size(160)
                .x(200)
                .y(100)
                .vtitle("RMSE")
                .utitle("Temp");
            select('#legend_g').append('g')
                .call(legend)
            let res_data = [];
            for (let i in HeatSumData) {
                console.log(HeatSumData[i])
                for (let j in HeatSumData[i]) {
                    // console.log(lineData[i][j].skip)
                    // console.log(timeScale(HeatSumData[i][j].skip), timeScale(HeatSumData[i][j].time), maxTime)
                    HeatSumData[i][j].x = timeScale(HeatSumData[i][j].time);
                    HeatSumData[i][j].w = timeScale(HeatSumData[i][j].skip) - timeScale(0);
                    HeatSumData[i][j].v = rmseScale(HeatSumData[i][j].rmse);
                    HeatSumData[i][j].id_cnt = HeatSumData[i][j].id_cnt;
                    HeatSumData[i][j].h = height / allStrip - 3
                    HeatSumData[i][j].y = parseInt(HeatSumData[i][j].id) * height / allStrip;
                    HeatSumData[i][j].rmseColor = heatColor(HeatSumData[i][j].v);
                    // HeatSumData[i][j].errorColor = heatColor(errorScale(HeatSumData[i][j].errorData));
                    // HeatSumData[i][j].rawColor = heatColor(rawScale(HeatSumData[i][j].rawData));
                    HeatSumData[i][j].corrColor = heatColor(corrScale(HeatSumData[i][j].corr));
                    HeatSumData[i][j].vsupColor1 = heatScale1(HeatSumData[i].rmse, HeatSumData[i][j].aveTemp);
                    // HeatSumData[i][j].vsupColor1 = heatScale1((HeatSumData[i][j].errorData), (HeatSumData[i][j].rawData));
                    HeatSumData[i][j].vsupColor2 = heatScale2((HeatSumData[i][j].rmse), HeatSumData[i][j].corr);
                    HeatSumData[i][j].colorMap = ['grey', 'grey', 'grey', HeatSumData[i][j].vsupColor2, HeatSumData[i][j].rmseColor, HeatSumData[i][j].corrColor];
                    // if (j == 10)
                    // break
                }
                res_data.push({
                    h: height / allStrip,
                    heat: HeatSumData[i]
                })
                // break
            }
            console.log(res_data)
            return res_data;
        },
        paintTimeScale: function(timestamp) {
            let margin = ({ top: 20, right: 10, bottom: 30, left: 50 });
            let timeData = [];
            timeData = [new Date(timestamp.start), new Date(timestamp.end)]
            let timeScale = scaleUtc(timeData, [margin.left, this.tlWidth - margin.right]);
            // this.timeScaleGlobal = timeScale;
            select('#representationTime').append('g').attr('id', 'representationTime_g').attr('transform', 'translate(0, 5)')
                .call(axisBottom(timeScale).ticks((this.tlWidth - margin.left - margin.right) / 40).tickSizeOuter(0))
        }
    },
    created() {},
    mounted() {
        this.elHeight = this.$refs.DataTransformation.offsetHeight;
        this.elWidth = this.$refs.DataTransformation.offsetWidth;
        this.tlHeight = this.$refs.RepresentationTimeAxis.offsetHeight;
        this.tlWidth = this.$refs.RepresentationTimeAxis.offsetWidth;

        this.paintTimeScale(this.allTimeScale[this.dataSelect])

        // const importData = import.meta.globEager('../assets/allData/multivariate_data/result_data/*.csv');
        // console.log(importData)
        // let multiDataSet = []
        // // for (let i in importData) {
        // //     multiDataSet.push(importData[i]['default'])
        // // }
        // console.log(multiDataSet)

        // this.heatRectData = this.calcRMSEHeat(dataSet, smoothDataSet, SN_raw_data, this.elWidth, this.elHeight - 0);


        const dataStore = useDataStore();
        let _this = this;
        // let dataSet = [d0, d1, d2, d3, d4, d5, d6, d7, d8, d9, d10, d11, d12, d13, d14, d15, d16, d17, d18, d19, d20, d21, d22, d23, d24, d25, d26, d27, d28, d29, d30, d31, d32, d33, d34, d35];
        // // console.log(dataSet);
        // this.heatRectData = this.calcRMSEHeatMultiVariable(dataSet, this.elWidth, this.elHeight);
        // let dataSet = [m0, m1, m2, m3, m4, m5, m6, m7, m8, m9, m10, m11, m12, m13, m14, m15, m16, m17, m18, m19, m20, m21, m22, m23, m24, m25, m26, m27];
        // let multi_smooth_data = [ms0, ms1, ms2, ms3, ms4, ms5, ms6];
        // let smooth_map = [0, 0, 6, 6, 0, 3, 6, 6, 3, 3, 3, 0, 1, 1, 1, 1, 2, 5, 4, 4, 2, 2, 4, 5, 5, 4, 5, 2];
        // let smooth_data = [];
        // for (let i of smooth_map) {
        //     smooth_data.push(multi_smooth_data[i]);
        // }

        // console.log(dataSet);
        // this.heatRectData = this.calcRMSETempHeatMultiVariate(dataSet, smooth_data, this.elWidth, this.elHeight);
        let dataSet = [rs1, rs2, rs3, rs4, rs5]

        // console.log(this.heatRectData);
        this.heatRectData = this.calcRMSEHeatMultiVariable(dataSet, this.elWidth, this.elHeight);

        dataStore.$subscribe((mutation, state) => {
            // if (dataStore.dataSelect == 'sunspots') {
            //     let dataSet = [d0, d1, d2, d3, d4, d5, d6, d7, d8, d9, d10, d11, d12, d13, d14, d15, d16, d17, d18, d19, d20, d21, d22, d23, d24, d25, d26, d27, d28, d29, d30, d31, d32, d33, d34, d35];
            //     // console.log(dataSet);
            //     this.heatRectData = this.calcRMSEHeatMultiVariable(dataSet, this.elWidth, this.elHeight);
            // } else {
            //     let dataSet = [m0, m1, m2, m3, m4, m5, m6, m7, m8, m9, m10, m11, m12, m13, m14, m15, m16, m17, m18, m19, m20, m21, m22, m23, m24, m25, m26, m27];
            //     // console.log(dataSet);
            //     this.heatRectData = this.calcRMSEHeatMultiVariable(dataSet, this.elWidth, this.elHeight);
            // }

                if (this.selectDot.tag != dataStore.selectDot.tag) {
                    this.selectDot.tag = dataStore.selectDot.tag;
                    this.selectDot.data = dataStore.selectDot.data;
                    let coverRect = [];
                    for (let i in this.heatRectData) {
                        for (let j in this.heatRectData[i].heat) {
                            if (this.selectDot.data[this.heatRectData[i].heat[j].uid]) {
                                coverRect.push(this.heatRectData[i].heat[j]);
                            }
                        }
                    }
                    this.coverRect = coverRect;
                }
            //     selectAll('#tsr_1').attr('opacity', 0);
            //     let coverRect = [];
            //     for (let i in _this.heatRectData) {
            //         for (let j in _this.heatRectData[i].heat) {
            //             // console.log(_this.heatRectData[i][j])

            //             if (dataStore.selectDot[this.heatRectData[i].heat[j].id_cnt] == 1) {
            //                 // select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('opacity', 1).attr('stroke', 'blue').attr('stroke-width', 0);
            //                 coverRect.push({
            //                     x: select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('x'),
            //                     y: select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('y'),
            //                     w: select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('width'),
            //                     h: select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('height'),
            //                     fill: select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('fill'),
            //                     cnt: select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('class'),
            //                 })
            //             }
            //             // else select('#tsr' + this.heatRectData[i].heat[j].id_cnt).attr('opacity', 0)
            //             // .attr('fill', '#d9d9d9');
            //         }
            //     }
            //     console.log(coverRect);
            //     this.coverRect = coverRect;
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

.el-button {
    border: 0px;
}

.representationTime_g {
    font-size: 14px;
}

.tick {
    font-size: 14px;
}
</style>
