<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-03-11 15:16:52
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Distribution View</div>
        <p class="titleTriangle"></p>
        <span style="position: absolute; top: 10px; right: 170px;">
            Pm 2.5: &nbsp;<img style="position: absolute;" src="../assets/img/Blues.png" alt="" width="150" height="20">
        </span>
    </div>
    <div class="frameworkBody">
        <div ref="DistributionView" style="height: 100%; width: 100%;">
            <!-- <img src="../assets/img/a.png" alt="" style="height: 100%;"> -->

            <svg id="distributionSVG" height="100%" width="100%">
                <g>
                    <g v-for="(item, i) in sparkBoxData" :key="'box' + i">
                        <rect x="10" y="10" fill="red"></rect>
                        <rect :x="item.rect1.x" :y="item.rect1.y" :width="item.rect1.w" :height="item.rect1.h"
                            fill="#f2f5fa">
                        </rect>
                        <rect :x="item.rect2.x" :y="item.rect2.y" :width="item.rect2.w" :height="item.rect2.h"
                            fill="#dce3f3">
                        </rect>
                        <path :d="'M ' + item.line.x1 + ' ' + item.line.y + ' L ' + item.line.x2 + ' ' + item.line.y"
                            :fill="'none'" :stroke="'#6d70b6'" stroke-width="3"></path>
                    </g>
                <g>
                    <path :d="linePath" stroke="steelblue" fill="none"></path>
                </g>
            </g>

            <g>
                <g>
                        <text v-for="(o, i) in S_name" :key="'F_leg' + i" :x="0" :y="0" text-anchor="end"
                            :transform="translate(40, 50 + i * (elHeight - 30) / S_name.length, -65)">{{
                                (o).charAt(0).toUpperCase() + (o).slice(1) }}</text>
                    </g>
                    <g>
                        <text v-for="(o, i) in S_name" :key="'F_leg' + i" :x="0" :y="0"
                            :transform="translate(50 + (elWidth - 50) / S_name.length / 2 + (elWidth - 50) / S_name.length * i, (elHeight - 30) + 20, 0)"
                            font-weight="100" text-anchor="middle">{{ (o).charAt(0).toUpperCase() + (o).slice(1) }}</text>
                    </g>
                    <g v-for="(o, i) in F_sparkBoxData" :key="'fsb' + i" :transform="translate(o.tx, o.ty, 0)">
                        <g :transform="translate(50, 15, 0)">
                            <rect v-for="(oo, r_i) in o.boxData" :key="'fsbr' + r_i" :x="oo.x" :y="oo.y" :width="oo.w"
                                :height="oo.h" :fill="oo.fillColor" :opacity="oo.fill" stroke="white"></rect>
                        </g>
                        <rect :x="o.rx" :y="o.ry" :width="o.w" :height="o.h" fill="none" stroke="black"></rect>
                    </g>
                    </g>
                <!-- <g>
                    <g>
                        <text v-for="(o, i) in F_name" :key="'F_leg' + i" :x="0" :y="0" text-anchor="end"
                            :transform="translate(40, 50 + i * (elHeight - 30) / F_name.length, -65)">{{
                                (o).charAt(0).toUpperCase() + (o).slice(1) }}</text>
                    </g>
                    <g>
                        <text v-for="(o, i) in F_name" :key="'F_leg' + i" :x="0" :y="0"
                            :transform="translate(50 + (elWidth - 50) / F_name.length / 2 + (elWidth - 50) / F_name.length * i, (elHeight - 30) + 20, 0)"
                            font-weight="100" text-anchor="middle">{{ (o).charAt(0).toUpperCase() + (o).slice(1) }}</text>
                    </g>
                    <g v-for="(o, i) in F_sparkBoxData" :key="'fsb' + i" :transform="translate(o.tx, o.ty, 0)">
                        <g :transform="translate(50, 15, 0)">
                            <rect v-for="(oo, r_i) in o.boxData" :key="'fsbr' + r_i" :x="oo.x" :y="oo.y" :width="oo.w"
                                :height="oo.h" :fill="oo.fillColor" :opacity="oo.fill" stroke="white"></rect>
                        </g>
                        <rect :x="o.rx" :y="o.ry" :width="o.w" :height="o.h" fill="none" stroke="black"></rect>
                    </g>
                </g> -->
            </svg>
        </div>
    </div>
</template>
<script>
import { max, min, sum } from "d3-array";
import { axisBottom, axisLeft } from "d3-axis";
import { scaleLinear, scaleOrdinal } from "d3-scale";
import { select } from "d3-selection";
import { line } from "d3-shape";
// import SN_raw_data from "../assets/SN_m_tot_V2.0.csv";
// import weight13 from '../assets/data/SN_weighted_moving_average13_tot.csv';
// import rolling13 from '../assets/data/SN_rolling13_tot.csv';


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
import multi_data from "../assets/used_multi.csv";
import { interpolateYlGnBu } from "d3-scale-chromatic";
export default {
    name: 'UnitView',
    props: [],
    data () {
        return {
            elHeight: 0,
            elWidth: 0,
            sparkBoxData: [],
            F_sparkBoxData: [],
            linePath: null,
            F_name: ['pm25', 'temp', 'rh', 'psfc', 'wnd_dir', 'wnd_spd'],
            S_name: ['raw', 'weight13', 'rolling13', 'weight3', 'rolling3', 'weight6', 'rolling6'],
            tfData: []
        }
    },
    methods: {
        translate (x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        },

        calcSparkBox (data, height, width) {
            let margin = ({ top: 10, right: 30, bottom: 20, left: 30 });
            // let height = 440;
            // let width = 1000;
            let focusHeight = 0;

            let y = scaleLinear()
                .domain([0, max(data, d => parseFloat(d.value))])
                .range([height - margin.bottom, height - margin.bottom - (width - margin.left - margin.right)]);
            let y2 = scaleLinear()
                .domain([0, max(data, d => parseFloat(d.value))])
                .range([focusHeight, margin.top])
            let x = scaleLinear()
                .domain([0, max(data, d => parseInt(d.id))])
                .range([margin.left, width - margin.right])
            let sparkBoxData = [];
            let lineData = [];
            let timeData = [];
            let lenData = [];
            for (let i = 0; i < data.length; ++i) {
                lineData.push({
                    id: parseInt(data[i].id),
                    v: parseFloat(data[i].value)
                });
                if (i == 0 || i == data.length - 1 || i % Math.floor(data.length / 10) === 0) {
                    lenData.push(x(parseInt(data[i].id)));
                    timeData.push(parseInt(data[i].timestamp));
                }
            }
            let timeScale = scaleOrdinal(timeData, lenData);
            let lineGenerate = line().x(d => x(d.id)).y(d => y(d.v));
            // console.log(timeData);
            select('#distributionSVG').append('g').call(axisBottom(timeScale)).attr('transform', `translate(${0}, ${this.elHeight - margin.bottom})`);
            select('#distributionSVG').append('g').call(axisLeft(y)).attr('transform', `translate(${margin.left}, ${0})`);

            for (let i = 0; i < data.length; i += Math.round(data.length / 20)) {
                // let tempValue = Array.from(new Set(data.slice(i, i + 10).map(d => parseFloat(d.value)).sort((a, b) => a - b)));
                let tempValue = data.slice(i, i + Math.round(data.length / 20)).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                let sumData = sum(tempValue);

                // console.log(sumData)

                // console.log(tempValue[tempValue.length /2 - 1], tempValue.length /2 - 1);
                sparkBoxData.push({
                    rect1: {
                        x: x(parseInt(data[i].id)),
                        y: y(tempValue[tempValue.length - 1]),
                        w: Math.abs(x(parseInt(data[i + ((i + Math.round(data.length / 20) < data.length) ? Math.round(data.length / 20) : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                        h: Math.abs(y(tempValue[0]) - y(tempValue[tempValue.length - 1]))
                    },
                    rect2: {
                        x: x(parseInt(data[i].id)),
                        y: y(tempValue[parseInt(tempValue.length * 3 / 4) - 1]),
                        w: Math.abs(x(parseInt(data[i + ((i + Math.round(data.length / 20) < data.length) ? Math.round(data.length / 20) : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                        h: Math.abs(y(tempValue[parseInt(tempValue.length * 3 / 4) - 1]) - y(tempValue[parseInt(tempValue.length / 4) - 1]))
                    },
                    line: {
                        x1: x(parseInt(data[i].id)),
                        // y: y(tempValue[parseInt(tempValue.length /2) - 1]),
                        // y: y((tempValue[0] + tempValue[parseInt(tempValue.length - 1)]) / 2),
                        y: y(sumData / tempValue.length),
                        x2: Math.abs(x(parseInt(data[i + ((i + Math.round(data.length / 20) < data.length) ? Math.round(data.length / 20) : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))) + x(parseInt(data[i].id))
                    }
                })
            }
            // console.log(sparkBoxData)
            return [sparkBoxData, lineGenerate(lineData)];
        },

        calcDisSparkBox (data, height, width, box_num, F1_name, F2_name, Val_name) {
            // console.log(Val_name, F1_name, F2_name)
            let margin = ({ top: 0, right: 0, bottom: 0, left: 0 });

            let y = scaleLinear()
                .domain([min(data, d => parseFloat(d[F2_name])), max(data, d => parseFloat(d[F2_name]))])
                .range([height - margin.bottom, margin.top])
            let x = scaleLinear()
                .domain([min(data, d => parseFloat(d[F1_name])), max(data, d => parseInt(d[F1_name]))])
                .range([margin.left, width - margin.right])
            let xx = scaleLinear()
                .domain([0, box_num])
                .range([margin.left, width - margin.right])

            let sparkBoxData = [];
            let timeGap = {};
            let timeG = (x.domain()[1] - x.domain()[0]) / (box_num);
            let valG = (y.domain()[1] - y.domain()[0]) / (box_num);
            for (let i = 0; i < data.length; ++i) {
                // console.log(data[i], data[i][F1_name], data[i][F2_name])
                let tx = Math.floor((data[i][F1_name] - x.domain()[0]) / timeG);
                if (tx == box_num) tx--;
                let ty = Math.floor((data[i][F2_name] - y.domain()[0]) / valG);
                if (ty == box_num) ty--;
                // console.log(tx, ty)
                if (typeof timeGap[tx] === 'undefined') {
                    timeGap[tx] = {};
                }
                if (typeof timeGap[tx][ty] === 'undefined') {
                    timeGap[tx][ty] = [];
                }
                timeGap[tx][ty].push(data[i]);
            }
            let maxV = -999999;
            let minV = 999999;
            // console.log(timeGap);
            for (let i in timeGap) {
                // console.log(i, timeGap[i])
                for (let j in timeGap[i]) {
                    // console.log(timeGap[i][j]);
                    let vSumData = sum(timeGap[i][j], d => parseFloat(d[Val_name]));
                    let yMax = max(timeGap[i][j], d => parseFloat(d[F2_name]));
                    let yMin = min(timeGap[i][j], d => parseFloat(d[F2_name]));
                    minV = Math.min(minV, vSumData / timeGap[i][j].length);
                    maxV = Math.max(maxV, vSumData / timeGap[i][j].length);
                    sparkBoxData.push({
                        x: xx(i),
                        w: xx(1) - xx(0),
                        y: y(yMax),
                        h: y(yMin) - y(yMax),
                        v: vSumData / timeGap[i][j].length
                    })
                }
            }
            let v = scaleLinear()
                .domain([minV, maxV])
                .range([0.3, 1]);
            let vScale = scaleLinear()
                .domain([minV, maxV])
                .range([0.3, 1]);
            

            for (let i in sparkBoxData) {
                sparkBoxData[i].fill = v(sparkBoxData[i].v);
                sparkBoxData[i].fillColor = interpolateYlGnBu(vScale(sparkBoxData[i].v));
            }
            return sparkBoxData;
        },
    },
    created () {
    },
    mounted () {
        this.elHeight = this.$refs.DistributionView.offsetHeight;
        this.elWidth = this.$refs.DistributionView.offsetWidth;
        // [this.sparkBoxData, this.linePath] = this.calcSparkBox(SN_raw_data, this.elHeight, this.elWidth);

        let F_sparkBoxData = []
        let margin = { top: 15, left: 50, right: 5, bottom: 30 }
        // console.log(rolling13, weight13, SN_raw_data);
        for (let i in SN_raw_data) {
            SN_raw_data[i]['weight13'] = sa13[i]['value'];
            SN_raw_data[i]['rolling13'] = sr13[i]['value'];
            SN_raw_data[i]['weight3'] = sa3[i]['value'];
            SN_raw_data[i]['rolling3'] = sr3[i]['value'];
            SN_raw_data[i]['weight6'] = sa6[i]['value'];
            SN_raw_data[i]['rolling6'] = sr6[i]['value'];
            SN_raw_data[i]['raw'] = SN_raw_data[i]['value'];
        }
        // console.log(SN_raw_data);

        for (let i = 0; i < this.S_name.length; ++i) {
            for (let j = 0; j < i + 1; ++j) {
                // console.log(this.S_name[i], this.S_name[j]);
                F_sparkBoxData.push({
                    x: j,
                    y: i,
                    tx: (this.elWidth - margin.left - margin.right) / this.S_name.length * j,
                    rx: margin.left,
                    w: (this.elWidth - margin.left - margin.right) / this.S_name.length,
                    ty: (this.elHeight - margin.bottom - margin.top) / this.S_name.length * i,
                    ry: margin.top,
                    h: (this.elHeight - margin.bottom - margin.top) / this.S_name.length,
                    boxData: this.calcDisSparkBox(SN_raw_data, (this.elHeight - margin.bottom - margin.top) / this.S_name.length, (this.elWidth - margin.left - margin.right) / this.S_name.length, 8, this.S_name[i], this.S_name[j], 'value')
                })
            }
        }

        // for (let i = 0; i < this.F_name.length; ++i) {
        //     for (let j = 0; j < i + 1; ++j) {
        //         F_sparkBoxData.push({
        //             x: j,
        //             y: i,
        //             tx: (this.elWidth - margin.left - margin.right) / this.F_name.length * j,
        //             rx: margin.left,
        //             w: (this.elWidth - margin.left - margin.right) / this.F_name.length,
        //             ty: (this.elHeight - margin.bottom - margin.top) / this.F_name.length * i,
        //             ry: margin.top,
        //             h: (this.elHeight - margin.bottom - margin.top) / this.F_name.length,
        //             boxData: this.calcDisSparkBox(multi_data, (this.elHeight - margin.bottom - margin.top) / this.F_name.length, (this.elWidth - margin.left - margin.right) / this.F_name.length, 8, this.F_name[i], this.F_name[j], 'pm25')
        //         })
        //     }
        // }
        // console.log(F_sparkBoxData);
        this.F_sparkBoxData = F_sparkBoxData;
    },
}
</script>
<style scoped>
*,
*::before,
*::after {
    font-weight: bold;
}
</style>
