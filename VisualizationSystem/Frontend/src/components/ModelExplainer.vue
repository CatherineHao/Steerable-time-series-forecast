<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-03-13 15:46:08
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Model Explainer</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="modelExplainer" :style="{
                    height: '100%',
                    width: elHeight + 'px',
                    float: 'right'
                }">
            <svg id="modelExplainer" height="100%" width="100%">
                    <g id="axis_g">
                            <g id="x_axis_g" :transform="translate(0, elHeight - 18, 0)"></g>
                            <g id="y_axis_g" :transform="translate(30, 0, 0)"></g>
                        </g>
                        <g id="scatter">
                            <!-- <circle v-for="(o, i) in dot_data" :key="'cir' + i" class="corr_cir" :id="'corr_cir' + o.id" :cx="o.x" :cy="o.y" :r="1"
                                                    fill="orange"></circle> -->
                        </g>
                        <g id="legend_g_s"></g>
                    </svg>
        </div>
        <div ref="modelTable" :style="{
                    height: '100%',
                    width: `calc(100% - ${elHeight}px - 10px)`,
                    float: 'left',
                    overflow: 'auto',
                    'font-size': '18px'
                }">
            <el-table :data="tableData" style="width: 100%" height="100%" :header-cell-style="{ 'text-align': 'center', 'font-size': '16px', 'background-color': 'rgba(250, 250, 250, 1)' }" :cell-style="{ 'text-align': 'center', 'font-size': '16px', 'height': '15px' }"
                :row-style="{ 'height': '18px' }">
                <!-- <el-table-column label="ID" prop="id" sortable /> -->
                <el-table-column label="Smooth" prop="smooth" />
                <el-table-column label="Skip" prop="skip" sortable />
                <el-table-column label="RMSE" prop="rmse" sortable />
                <el-table-column label="Corr." prop="norm_corr" sortable />
            </el-table>
        </div>
    
    </div>
</template>

<script>
import { scaleLinear } from 'd3-scale';
import { useDataStore } from "../stores/counter";
import average6Data from "../assets/average6_slice_info.json";
import { max, min } from 'd3-array';
import res_data from '../assets/data/model_skip_results.json'

// data import
import d0 from '../assets/explaindata/rawdata_skip13_0.8.csv';
import d1 from '../assets/explaindata/rawdata_skip1_0.8.csv';
import d2 from '../assets/explaindata/rawdata_skip3_0.8.csv';
import d3 from '../assets/explaindata/rawdata_skip6_0.8.csv';
import d4 from '../assets/explaindata/rolling13_skip13_0.8.csv';
import d5 from '../assets/explaindata/rolling13_skip1_0.8.csv';
import d6 from '../assets/explaindata/rolling13_skip3_0.8.csv';
import d7 from '../assets/explaindata/rolling13_skip6_0.8.csv';
import d8 from '../assets/explaindata/rolling3_skip13_0.8.csv';
import d9 from '../assets/explaindata/rolling3_skip1_0.8.csv';
import d10 from '../assets/explaindata/rolling3_skip3_0.8.csv';
import d11 from '../assets/explaindata/rolling3_skip6_0.8.csv';
import d12 from '../assets/explaindata/rolling6_skip13_0.8.csv';
import d13 from '../assets/explaindata/rolling6_skip1_0.8.csv';
import d14 from '../assets/explaindata/rolling6_skip3_0.8.csv';
import d15 from '../assets/explaindata/rolling6_skip6_0.8.csv';
import d16 from '../assets/explaindata/rolling9_skip13_0.8.csv';
import d17 from '../assets/explaindata/rolling9_skip1_0.8.csv';
import d18 from '../assets/explaindata/rolling9_skip3_0.8.csv';
import d19 from '../assets/explaindata/rolling9_skip6_0.8.csv';
import d20 from '../assets/explaindata/weighted13_skip13_0.8.csv';
import d21 from '../assets/explaindata/weighted13_skip1_0.8.csv';
import d22 from '../assets/explaindata/weighted13_skip3_0.8.csv';
import d23 from '../assets/explaindata/weighted13_skip6_0.8.csv';
import d24 from '../assets/explaindata/weighted3_skip13_0.8.csv';
import d25 from '../assets/explaindata/weighted3_skip1_0.8.csv';
import d26 from '../assets/explaindata/weighted3_skip3_0.8.csv';
import d27 from '../assets/explaindata/weighted3_skip6_0.8.csv';
import d28 from '../assets/explaindata/weighted6_skip13_0.8.csv';
import d29 from '../assets/explaindata/weighted6_skip1_0.8.csv';
import d30 from '../assets/explaindata/weighted6_skip3_0.8.csv';
import d31 from '../assets/explaindata/weighted6_skip6_0.8.csv';
import d32 from '../assets/explaindata/weighted9_skip13_0.8.csv';
import d33 from '../assets/explaindata/weighted9_skip1_0.8.csv';
import d34 from '../assets/explaindata/weighted9_skip3_0.8.csv';
import d35 from '../assets/explaindata/weighted9_skip6_0.8.csv';
import { axisBottom, axisLeft } from 'd3-axis';
// import time from 'd3-scale/src/time';
import { select, selectAll } from 'd3-selection';
import { line } from 'd3-shape';
import { drag } from 'd3-drag';
import { polygonContains } from 'd3-polygon';

import * as vsup from 'vsup';
import { interpolateYlOrRd } from 'd3-scale-chromatic';
export default {
    name: 'modelExplainerView',
    props: ['sliceData'],
    data() {
        return {
            elHeight: 0,
            elWidth: 0,
            skip_length: [13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6],
            dot_data: [],
            tableData: [],
            DataTransferItem
        }
    },
    methods: {
        setupLasso() {
            let _this = this;
            let lasso_g = select("#scatter")
                .append('g')
                .attr('class', 'lasso');
            let origin_node = lasso_g
                .append("circle")
                .attr("id", "origin");
            let draw_path = lasso_g
                .append('path')
                .attr("id", 'drawn');
            let close_path = lasso_g
                .append("path")
                .attr("id", "loop_close");

            let select_path = "";
            // let end_path = "";
            // let origin_circle;
            let target_circle;
            let closePathDistance = 100;

            let polygon = new Array();
            let lassoPoint = new Array();

            let dragStarted = function(event) {
                event
                _this.lasso_t = 0;
            }
            let dragged = function(event) {
                let tx = event.x;
                let ty = event.y;
                if (select_path == "") {
                    select_path = select_path + "M " + tx + " " + ty;
                    target_circle = [event.x, event.y];
                    polygon.push([event.x, event.y]);
                } else {
                    select_path = select_path + "L " + tx + " " + ty;
                }
                polygon.push([tx, ty]);
                let distance = Math.sqrt(Math.pow(tx - target_circle[0], 2) + Math.pow(ty - target_circle[1], 2));
                let close_draw_path = "M " + tx + " " + ty + " L " + target_circle[0] + " " + target_circle[1];
                draw_path.attr('d', select_path);

                close_path.attr("d", close_draw_path);
                if (distance < closePathDistance) {
                    close_path.attr("display", null);
                } else {
                    close_path.attr("display", "none");
                }
            }
            let dragEnded = async function(event) {
                origin_node.attr("display", "none");
                // draw_path.attr("d", null);
                // close_path.attr("d", null);
                let tx = event.x;
                let ty = event.y;
                if (select_path == "") {
                    select_path = select_path + "M " + tx + " " + ty;
                    target_circle = [event.x, event.y];
                    polygon.push([event.x, event.y]);
                } else {
                    select_path = select_path + "L " + tx + " " + ty;
                }

                let distance = Math.sqrt(Math.pow(tx - target_circle[0], 2) + Math.pow(ty - target_circle[1], 2));

                if (distance < 10)
                    return;

                _this.lasso_t = 1;
                let select_dot = new Object();
                let select_info = new Array();
                let selectSkip = new Array();
                for (let i in _this.dot_data) {
                    let dot_p = [_this.dot_data[i].x, _this.dot_data[i].y];
                    if (polygonContains(polygon, dot_p)) {
                        select_dot[i] = 1;
                        select_info.push(1);
                        // console.log('#tsr' + i)
                        // select('#tsr' + i).attr("opacity", d => {
                        //     console.log(d);
                        //     selectSkip.push(d);
                        //     return 0;
                        // })
                        // console.log
                        // cie_x += parseFloat(_this.poem_dot[i].raw_value.x);
                        // cie_y += parseFloat(_this.poem_dot[i].raw_value.y);
                        // cie_cnt += 1;
                    } else {
                        select_info.push(0);
                    }
                }
                // console.log(selectSkip);

                // console.log(select_dot);
                const dataStore = useDataStore();
                dataStore.selectDot = select_dot;
                selectAll('.rst').attr('fill', '#bbb').attr('fill-opacity', 0.5)


                _this.tableData = _this.calcTableData(_this.dataSet, select_dot);
                selectAll('.corr_cir').attr('opacity', (d, i) => {
                    if (select_dot[i] == 1) return 1;
                    else return d.isShow == 0 ? 0 : 0.5;
                }).attr('fill', (d, i) => {
                    if (select_dot[i] == 1) return d.fill;
                    else return '#d9d9d9';
                })

                select_path = "";
                // end_path = "";
                // origin_circle = [];
                target_circle = [];
                closePathDistance = 100;
                polygon = new Array();
            }

            let dragL = drag()
                .on('start', dragStarted)
                .on('drag', dragged)
                .on('end', dragEnded);
            // console.log(drag);

            select('#modelExplainer').call(dragL);
        },
        translate(x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        },
        formatNum(num) {
            //1. 可能是字符串，转换为浮点数
            //2. 乘以100 小数点向右移动两位
            //3. Math.round 进行四舍五入
            //4. 除以100 小数点向左移动两位 实现保留小数点后两位
            let v = Math.round(parseFloat(num) * 100) / 100;
            // 去掉小数点 存为数组
            let arrayNum = v.toString().split(".");
            //只有一位（整数）
            if (arrayNum.length == 1) {
                return v.toString() + ".00";
            }
            if (arrayNum.length > 1) {
                //小数点右侧 如果小于两位 则补一个0
                if (arrayNum[1].length < 2) {
                    return v.toString() + "0";
                }
                return v;
            }
        },
        calcTableData(data, select_dot) {
            let sdata = [];
            let id_cnt = 0;
            for (let i in data) {
                let startPos = 840;
                let tp = [];
                if (i > 10) break;
                // if (i >= 16 && i <= 19)
                //     continue;
                for (let j in data[i]) {
                    // console.log(data[i][j])
                    if (j > 1000) break;
                    if (parseFloat(data[i][j]['norm_corr']) == 0)
                        continue;

                    id_cnt++;
                    let predict_data = data[i][j]['predict_data'].split(' ');
                    let pre_data = [];
                    for (let k in predict_data) {
                        if (predict_data[k] == '' || predict_data[k] == '[' || predict_data[k] == ']') {
                            continue;
                        }
                        // console.log(predict_data[k])
                        if (predict_data[k][0] == '[') {
                            // console.log(predict_data[k].slice(1, -1))
                            pre_data.push(parseFloat(predict_data[k].slice(1, -1)));
                        } else if (predict_data[k][predict_data[k].length - 1] == ']') {
                            // console.log(predict_data[k].slice(0, -1))
                            pre_data.push(parseFloat(predict_data[k].slice(0, -1)));
                        } else if (predict_data[k][predict_data[k].length -1] == '\n') {
                            // console.log(predict_data[k])
                            pre_data.push(parseFloat(predict_data[k].slice(0, -1)));
                        } else {
                            pre_data.push(parseFloat(predict_data[k]));
                        }
                    }
                        // console.log(pre_data)
                    // console.log(Array.from(data[i][j]['predict_data']))
                    // if (select_dot[id_cnt] == 0) {
                    //     continue;
                    // }
                    // console.log(id_cnt);
                    sdata.push({
                        id: id_cnt,
                        predict_data: pre_data,
                        smooth: data[i][j]['smooth'],
                        skip: data[i][j]['skip'],
                        time: j * this.skip_length[i] + startPos,
                        norm_corr: this.formatNum(parseFloat(data[i][j]['norm_corr'])),
                        rmse: this.formatNum(parseFloat(data[i][j]['rmse']))
                    });
                    // tp.push({
                    //     id: i,
                    //     time: j * this.skip_length[i] + startPos,
                    //     rmse: parseFloat(data[i][j]['rmse'])
                    // });
                    // if (isNaN(data[i][j]['norm_corr']))
                    // console.log(i)
                    // maxTime = Math.max(maxTime, j * this.skip_length[i] + startPos);
                    // maxRmse = Math.max(maxRmse, parseFloat(data[i][j]['rmse']));
                    // minRmse = Math.min(minRmse, parseFloat(data[i][j]['rmse']));
                    // maxNorm = Math.max(maxNorm, parseFloat(data[i][j]['norm_corr']));
                    // minNorm = Math.min(minNorm, parseFloat(data[i][j]['norm_corr']));
                }
                // lineData.push(tp);
            }
            // sdata.sort((a, b) => {
            //     if (a.rmse != b.rmse) return b.rmse - a.rmse;
            //     return b.norm_corr - a.norm_corr;
            // })
            // sdata = sdata.slice(0, 200);

            return sdata;
        },
        calcScatter(data) {
            // console.log(data)
            let sdata = [];
            let maxRmse = -999999;
            let minRmse = 999999;
            let maxNorm = -999999;
            let minNorm = 999999;
            let maxTime = -999999;
            let lineData = [];
            let id_cnt = 0;
            for (let i in data) {
                let startPos = 840;
                let tp = [];
                // if (i >= 16 && i <= 19)
                //     continue;
                for (let j in data[i]) {
                    // console.log(data[i][j])
                    if (parseFloat(data[i][j]['129_pearson']) == 0)
                        continue;
                    let className = (data[i][j]['smooth'] == 'raw' ? 'rawdata' : data[i][j]['smooth']) + '_skip' + data[i][j]['skip'];
                    sdata.push({
                        id: i,
                        time: j * this.skip_length[i] + startPos,
                        norm_corr: parseFloat(data[i][j]['129_pearson']),
                        rmse: parseFloat(data[i][j]['rmse']),
                        isShow: Math.random() < 0.1 ? 1 : 0,
                        id_cnt: id_cnt,
                        class_name: className
                    });
                    // tp.push({
                    //     id: i,
                    //     time: j * this.skip_length[i] + startPos,
                    //     rmse: parseFloat(data[i][j]['rmse'])
                    // });
                    // if (isNaN(data[i][j]['norm_corr']))
                    // console.log(i)
                    maxTime = Math.max(maxTime, j * this.skip_length[i] + startPos);
                    maxRmse = Math.max(maxRmse, parseFloat(data[i][j]['rmse']));
                    minRmse = Math.min(minRmse, parseFloat(data[i][j]['rmse']));
                    maxNorm = Math.max(maxNorm, parseFloat(data[i][j]['129_pearson']));
                    minNorm = Math.min(minNorm, parseFloat(data[i][j]['129_pearson']));

                    id_cnt++;
                }
                // lineData.push(tp);
            }
            let quantization2 = vsup.quantization().branching(2).layers(4).valueDomain([minRmse, maxRmse]).uncertaintyDomain([(maxNorm), minNorm]);
            let heatColor = interpolateYlOrRd;
            let heatScale = vsup.scale().quantize(quantization2).range(heatColor);
            // console.log(minNorm, maxNorm);

            let rmseScale = scaleLinear([minRmse, maxRmse], [this.elHeight - 18, 10]);
            let normScale = scaleLinear([minNorm, maxNorm], [30, this.elWidth - 20]);
            var legend = vsup.legend.arcmapLegend();

            legend
                .scale(heatScale)
                .size(120)
                .x(100)
                .y(40)
                .vtitle("RMSE")
                .utitle("Corr.");
            select('#legend_g_s').append('g')
                .call(legend)

            let timeScale = scaleLinear([840, maxTime], [30, this.elWidth - 20]);
            let xAxis = axisBottom(normScale).ticks(10);
            let yAxis = axisLeft(rmseScale).ticks(10);
            select("#x_axis_g").call(xAxis).call(g => g.selectAll(".title").data(['Corr.']).join("text")
                .attr("class", "title")
                .attr("x", this.elWidth - 20)
                .attr("y", 16)
                .attr("fill", "currentColor")
                .attr("text-anchor", "middle")
                .attr('font-size', '14px')
                .text('Corr.'));
            select("#y_axis_g").call(yAxis).call(g => g.selectAll(".title").data(['RMSE']).join("text")
                .attr("class", "title")
                .attr("x", 20)
                .attr("y", 12)
                .attr("fill", "currentColor")
                .attr("text-anchor", "middle")
                .attr('font-size', '14px')
                .text('RMSE'));
            for (let i in sdata) {
                sdata[i].x = normScale(sdata[i].norm_corr);
                sdata[i].y = rmseScale(sdata[i].rmse);
                sdata[i].fill = heatScale(sdata[i].rmse, sdata[i].norm_corr);
            }
            let _this = this;
            select('#scatter')
                .append('g')
                .selectAll('#res_c')
                .attr('id', 'res_c')
                .data(sdata, d => {
                    if (d.isShow == 1) {
                        return d;
                    }
                })
                .enter()
                .append('circle')
                .attr('cx', d => d.x)
                .attr('cy', d => d.y)
                .attr('id', (d, i) => 'corr_c' + d.id_cnt)
                .attr('class', 'corr_cir')
                .attr('r', 2)
                // .attr('stroke', '#bbb')
                .attr('fill', d => d.fill)
                .attr('opacity', d => d.isShow)
                .on('mouseover', (e, d) => {
                    select('#corr_c' + d.id_cnt).attr('r', d.isShow == 1 ? 5 : 1);
                })
                .on('mouseout', (e, d) => {
                    select('#corr_c' + d.id_cnt).attr('r', 2);
                })
                .on('click', (e, d) => {
                    // console.log()
                    let select_dot = [];
                    // select_dot[d.id_cnt] = 1;
                    for (let i = 0; i < id_cnt; ++i) {
                        select_dot.push(0);
                        if (i == d.id_cnt) {
                            console.log(d.id_cnt)
                            select_dot[i] = 1;
                        }
                    }
                    const dataStore = useDataStore();
                    dataStore.selectDot = select_dot;

                    // console.log(select_dot);
                    _this.tableData = _this.calcTableData(_this.dataSet, select_dot);
                    console.log(_this.tableData);
                    selectAll('.corr_cir').attr('opacity', (d, i) => {
                        if (select_dot[i] == 1) return 1;
                        else return d.isShow == 0 ? 0 : 0.5;
                    }).attr('fill', (d, i) => {
                        if (select_dot[i] == 1) return 'orange';
                        else return '#d9d9d9';
                    })
                })

            return sdata;
        }
    },
    created() {},
    mounted() {
        this.elHeight = this.$refs.modelExplainer.offsetHeight;
        // this.elWidth = this.$refs.modelExplainer.offsetWidth;
        this.elWidth = this.elHeight;
        // console.log(dataX);

        let dataSet = [d0, d1, d2, d3, d4, d5, d6, d7, d8, d9, d10, d11, d12, d13, d14, d15, d16, d17, d18, d19, d20, d21, d22, d23, d24, d25, d26, d27, d28, d29, d30, d31, d32, d33, d34, d35];
        this.dataSet = dataSet;

        this.dot_data = this.calcScatter(dataSet);
        this.tableData = this.calcTableData(dataSet);

        this.setupLasso();
    },
    watch: {

    }
}
</script>

<style>
*,
*::before,
*::after {
    font-weight: normal;
}

.cell {
    font-weight: normal;
    color: black;
}

.lasso path {
    stroke: #2378ae;
    /* stroke: white; */
    stroke-width: 3;
    stroke-dasharray: 4, 4;
}

.lasso #drawn {
    fill-opacity: 0.05;
}

.lasso #loop_close {
    fill: none;
    stroke-dasharray: 4, 4;
}

.lasso #origin {
    fill: rgb(180, 180, 180);
    fill-opacity: 0.5;
}

.el-table__cell {
    height: 15px;
}

/*chrome--------------------------------------------start*/

::-webkit-scrollbar {
    width: 8px;
    height: 8px;
}

/* Track */

::-webkit-scrollbar-track {
    background: #ffffff;
    border-radius: 8px;
}

/* Handle */

::-webkit-scrollbar-thumb {
    background: rgb(201, 201, 202);
    border-radius: 8px;
}

/* Handle on hover */

::-webkit-scrollbar-thumb:hover {
    background: rgb(162, 162, 163);
}

.el-menu::-webkit-scrollbar,
.el-table__body-wrapper::-webkit-scrollbar {
    display: none;
}

.el-menu:hover::-webkit-scrollbar,
.el-table__body-wrapper:hover::-webkit-scrollbar {
    display: block;
}

/*chrome--------------------------------------------end*/
</style>
