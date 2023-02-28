<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-02-18 20:36:39
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Model Explainer</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="modelTable"
            style="height: 100%; width: calc(50% - 7.5px); float: right; overflow:auto; font-size: 18px;">
            <el-table :data="tableData" style="width: 100%" height="100%"
                :header-cell-style="{ 'text-align': 'center', 'font-size': '16px', 'background-color': 'rgba(250, 250, 250, 1)' }"
                :cell-style="{ 'text-align': 'center', 'background-color': 'rgba(250, 250, 250, 1)', 'font-size': '16px' }">
                <!-- <el-table-column type="expand">
                    <template #default="props">
                        <div m="4">
                            <el-table :data="props.row['sub_slice']" stripe style="width: 100%; float: right;" height="200"
                                :table-layout="'auto'" :header-cell-style="{ 'text-align': 'center' }"
                                :cell-style="{ 'text-align': 'center' }">
                                <el-table-column label="ID" prop="slice_num" />
                                <el-table-column label="MAE" prop="MAE" />
                                <el-table-column label="STD" prop="STD" />
                                <el-table-column label="MEAN" prop="Mean" />
                                <el-table-column label="BEGIN" prop="train_begin" />
                                <el-table-column label="END" prop="test_end" />

                            </el-table>
                        </div>
                    </template>
                </el-table-column> -->
                <el-table-column label="Smooth" prop="smooth" />
                <el-table-column label="Skip" prop="skip" />
                <el-table-column label="RMSE" prop="rmse" />
                <el-table-column label="Corr." prop="norm_corr" />
            </el-table>
        </div>
        <div ref="modelExplainer" style="height: 100%; width: calc(50%); float: left;">
            <svg id="modelExplainer" height="100%" width="100%">
                <g id="axis_g">
                    <g id="x_axis_g" :transform="translate(0, elHeight - 18, 0)"></g>
                    <g id="y_axis_g" :transform="translate(30, 0, 0)"></g>
                </g>
                <g id="scatter">
                    <!-- <circle v-for="(o, i) in dot_data" :key="'cir' + i" class="corr_cir" :id="'corr_cir' + o.id" :cx="o.x" :cy="o.y" :r="1"
                            fill="orange"></circle> -->
                </g>
            </svg>
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

export default {
    name: 'modelExplainerView',
    props: ['sliceData'],
    data () {
        return {
            elHeight: 0,
            elWidth: 0,
            skip_length: [13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6],
            dot_data: [],
            tableData: []
        }
    },
    methods: {
        translate (x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        },
        formatNum (num) {
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
        calcTableData (data) {
            let sdata = [];
            for (let i in data) {
                let startPos = 840;
                let tp = [];
                if (i > 10) break;
                // if (i >= 16 && i <= 19)
                //     continue;
                for (let j in data[i]) {
                    // console.log(data[i][j])
                    if (parseFloat(data[i][j]['norm_corr']) == 0)
                        continue;
                    sdata.push({
                        id: i,
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
            sdata.sort((a, b) => {
                if (a.rmse != b.rmse) return b.rmse - a.rmse;
                return b.norm_corr - a.norm_corr;
            })
            sdata = sdata.slice(0, 200);

            return sdata;
        },
        calcScatter (data) {
            let sdata = [];
            let maxRmse = -999999;
            let minRmse = 999999;
            let maxNorm = -999999;
            let minNorm = 999999;
            let maxTime = -999999;
            let lineData = [];
            for (let i in data) {
                let startPos = 840;
                let tp = [];
                if (i >= 16 && i <= 19)
                    continue;
                for (let j in data[i]) {
                    // console.log(data[i][j])
                    if (parseFloat(data[i][j]['norm_corr']) == 0)
                        continue;
                    sdata.push({
                        id: i,
                        time: j * this.skip_length[i] + startPos,
                        norm_corr: parseFloat(data[i][j]['norm_corr']),
                        rmse: parseFloat(data[i][j]['rmse'])
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
                    maxNorm = Math.max(maxNorm, parseFloat(data[i][j]['norm_corr']));
                    minNorm = Math.min(minNorm, parseFloat(data[i][j]['norm_corr']));
                }
                // lineData.push(tp);
            }
            // console.log(minNorm, maxNorm);

            let rmseScale = scaleLinear([minRmse, maxRmse], [this.elHeight - 18, 10]);
            let normScale = scaleLinear([minNorm, maxNorm], [30, this.elWidth - 20]);

            let timeScale = scaleLinear([840, maxTime], [30, this.elWidth - 20]);
            let xAxis = axisBottom(normScale).ticks(10);
            let yAxis = axisLeft(rmseScale).ticks(10);
            select("#x_axis_g").call(xAxis);
            select("#y_axis_g").call(yAxis);
            for (let i in sdata) {
                sdata[i].x = normScale(sdata[i].norm_corr);
                sdata[i].y = rmseScale(sdata[i].rmse);
            }
            select('#scatter')
                .append('g')
                .selectAll('#res_c')
                .attr('id', 'res_c')
                .data(sdata)
                .enter()
                .append('circle')
                .attr('cx', d => d.x)
                .attr('cy', d => d.y)
                .attr('class', 'corr_cir')
                .attr('r', 1)
                .attr('fill', 'orange')



            // let lineGenerate = line().x(d => timeScale(d.time)).y(d => rmseScale(d.rmse));

            // let lres = [];
            // for (let i in lineData) {
            //     lres.push({
            //         d: lineGenerate(lineData[i]),
            //         id: i
            //     });
            // }

            // select('#scatter').append('g')
            //     .selectAll('#res_p')
            //     .attr('id', 'res_p')
            //     .data(lres)
            //     .enter()
            //     .append('path')
            //     .attr('class', 'p_x')
            //     .attr('d', d => d.d)
            //     .attr('fill', 'none')
            //     .attr('stroke', (d, i) => {
            //         // if (i == 10)
            //         return 'black'
            //         return 'none'
            //     })
            //     .on('mouseover', (e, d, i) => {
            //         // console.log(d, i);
            //         // console.log(d);

            //         select('#rst' + d.id).attr('stroke-width', 3);

            //         selectAll('.p_x').attr('opacity', (td, ti) => {
            //             // console.log(td);
            //             if (d.id == td.id) {
            //                 return 1;
            //             }
            //             return 0;
            //         })
            //     })
            //     .on('mouseout', (e, d, i) => {
            //         selectAll('.p_x').attr('opacity', 1)
            //         select('#rst' + d.id).attr('stroke-width', 0);

            //     })


            return sdata;
        }
    },
    created () {
    },
    mounted () {
        this.elHeight = this.$refs.modelExplainer.offsetHeight;
        this.elWidth = this.$refs.modelExplainer.offsetWidth;
        // console.log(dataX);
        let dataSet = [d0, d1, d2, d3, d4, d5, d6, d7, d8, d9, d10, d11, d12, d13, d14, d15, d16, d17, d18, d19, d20, d21, d22, d23, d24, d25, d26, d27, d28, d29, d30, d31, d32, d33, d34, d35];
        this.dot_data = this.calcScatter(dataSet);
        this.tableData = this.calcTableData(dataSet);


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
