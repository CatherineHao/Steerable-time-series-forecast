<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-02-02 15:10:22
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Model Explainer</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="modelTable"
            style="height: 100%; width: calc(50% - 7.5px + 30px); float: left; overflow:auto; font-size: 18px;">
            <el-table :data="tableData" style="width: 100%" height="100%"
                :header-cell-style="{ 'text-align': 'center', 'font-size': '16px', 'background-color': 'rgba(250, 250, 250, 1)' }"
                :cell-style="{ 'text-align': 'center', 'background-color': 'rgba(250, 250, 250, 1)', 'font-size': '16px' }">
                <el-table-column type="expand">
                    <template #default="props">
                        <div m="4">
                            <!-- <p m="t-0 b-2">State: {{ props.row.state }}</p>
                            <p m="t-0 b-2">City: {{ props.row.city }}</p>
                            <p m="t-0 b-2">Address: {{ props.row.address }}</p>
                            <p m="t-0 b-2">Zip: {{ props.row.zip }}</p>
                            <h3>Family</h3> -->
                            <el-table :data="props.row['sub_slice']" stripe style="width: 100%; float: right;"
                                height="200" :table-layout="'auto'" :header-cell-style="{ 'text-align': 'center' }"
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
                </el-table-column>
                <el-table-column label="Slice number" prop="slice_number" />
                <el-table-column label="Smooth" prop="smooth" />
            </el-table>
        </div>
        <div ref="modelExplainer" style="height: 100%; width: calc(50% - 7.5px - 30px); float: right;">
            <svg id="modelExplainer" height="100%" width="100%">

                <g>
                    <text v-if="runTag" font-weight="bold" x="15" y="15" font-size="20">Correlation</text>
                    <text v-if="runTag" font-weight="bold" :x="elWidth - 75" :y="elHeight - 35" font-size="20">RMSE</text>
                    <g v-for="(item, i) in xAxis">
                        <path :d="'M ' + item.x + ' ' + item.y + ' L ' + + item.x + ' ' + (item.y - 5)" stroke="black">
                        </path>
                        <path :d="'M ' + item.x + ' ' + item.y + ' L ' + + item.x + ' ' + (20)"
                            stroke="rgba(229, 229, 229)" stroke-dasharray="0"></path>
                        <text :x="item.x" :y="elHeight - 10" dx="-0.3em">{{ item.t }}</text>
                    </g>
                    <g v-for="(item, i) in yAxis">
                        <path :d="'M ' + (item.x + 25) + ' ' + item.y + ' L ' + + (item.x + 30) + ' ' + (item.y)"
                            stroke="black"></path>
                        <path :d="'M ' + (elWidth - 20) + ' ' + item.y + ' L ' + + (item.x + 30) + ' ' + (item.y)"
                            stroke="rgba(229, 229, 229)" stroke-dasharray="0"></path>
                        <text :x="0" :y="item.y" dy="0.3em">{{ item.t }}</text>
                    </g>
                </g>
                <g>
                    <circle v-for="(item, i) in nodeData" :cx="item.x" :cy="item.y" :r="3" fill="steelblue"></circle>
                </g>
            </svg>
        </div>
    </div>
</template>
<script>
import { scaleLinear } from 'd3-scale';
import { useDataStore } from "../stores/counter";
export default {
    name: 'modelExplainerView',
    props: ['sliceData'],
    data() {
        return {
            runTag: false,
            elHeight: 0,
            elWidth: 0,
            nodeData: [],
            xAxis: [],
            yAxis: [],
            tableData: [],
            tableDataB: [
                {
                    date: '2016-05-03',
                    name: 'Tom',
                    state: 'California',
                    city: 'San Francisco',
                    address: '3650 21st St, San Francisco',
                    zip: 'CA 94114',
                    family: [
                        {
                            name: 'Jerry',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Spike',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Tyke',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                    ],
                },
                {
                    date: '2016-05-02',
                    name: 'Tom',
                    state: 'California',
                    city: 'San Francisco',
                    address: '3650 21st St, San Francisco',
                    zip: 'CA 94114',
                    family: [
                        {
                            name: 'Jerry',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Spike',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Tyke',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                    ],
                },
                {
                    date: '2016-05-04',
                    name: 'Tom',
                    state: 'California',
                    city: 'San Francisco',
                    address: '3650 21st St, San Francisco',
                    zip: 'CA 94114',
                    family: [
                        {
                            name: 'Jerry',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Spike',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Tyke',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                    ],
                },
                {
                    date: '2016-05-01',
                    name: 'Tom',
                    state: 'California',
                    city: 'San Francisco',
                    address: '3650 21st St, San Francisco',
                    zip: 'CA 94114',
                    family: [
                        {
                            name: 'Jerry',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Spike',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Tyke',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                    ],
                },
                {
                    date: '2016-05-08',
                    name: 'Tom',
                    state: 'California',
                    city: 'San Francisco',
                    address: '3650 21st St, San Francisco',
                    zip: 'CA 94114',
                    family: [
                        {
                            name: 'Jerry',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Spike',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Tyke',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                    ],
                },
                {
                    date: '2016-05-06',
                    name: 'Tom',
                    state: 'California',
                    city: 'San Francisco',
                    address: '3650 21st St, San Francisco',
                    zip: 'CA 94114',
                    family: [
                        {
                            name: 'Jerry',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Spike',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Tyke',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                    ],
                },
                {
                    date: '2016-05-07',
                    name: 'Tom',
                    state: 'California',
                    city: 'San Francisco',
                    address: '3650 21st St, San Francisco',
                    zip: 'CA 94114',
                    family: [
                        {
                            name: 'Jerry',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Spike',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                        {
                            name: 'Tyke',
                            state: 'California',
                            city: 'San Francisco',
                            address: '3650 21st St, San Francisco',
                            zip: 'CA 94114',
                        },
                    ],
                },
            ]
        }
    },
    methods: {
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
        calcAxis() {
            const xScale = scaleLinear([0, 1], [30, this.elWidth - 20]);
            const yScale = scaleLinear([0, 1], [this.elHeight - 30, 20]);
            let xAxis = [];
            let yAxis = [];
            for (let i = 0; i <= 10; ++i) {
                xAxis.push({
                    t: i / 10,
                    x: xScale(i / 10),
                    y: this.elHeight - 25,
                    // d: 
                });
                yAxis.push({
                    t: i / 10,
                    y: yScale(i / 10),
                    x: 0
                });
            }
            return [xAxis, yAxis];
        },
        calcNode(data) {
            let nodeData = [];
            const xScale = scaleLinear([0, 1], [30, this.elWidth - 20]);
            const yScale = scaleLinear([0, 1], [this.elHeight - 30, 20]);
            for (let i = 0; i < 76; ++i) {
                let rx = Math.random();
                let ry = Math.random();
                nodeData.push({
                    x: xScale(rx),
                    y: yScale(ry),
                    r: [rx, ry]
                });
            }
            return nodeData;
        },
        calcTableData(data) {
            let tData = new Array();
            for (const d of data) {
                let td = [];
                for (let i in d['sub slice']) {
                    let tmpD = {}
                    for (let k in d['sub slice'][i]) {
                        let tmp = d['sub slice'][i][k]
                        if (k == "Mean" || k == "STD" || k == 'MAE') {
                            tmp = this.formatNum(tmp);
                        }
                        tmpD[k] = tmp;
                    }
                    td.push(tmpD);
                }
                let tdata = {
                    slice_number: d['slice_info']['slice_number'] + '-slice',
                    smooth: '13-Average',
                    sub_slice: td
                }
                tData.push(tdata);
            }
            return tData;
        }
    },
    created() {
    },
    mounted() {
        this.elHeight = this.$refs.modelExplainer.offsetHeight;
        this.elWidth = this.$refs.modelExplainer.offsetWidth;
        const dataStore = useDataStore();
        dataStore.$subscribe((mutation, state) => {
            this.runTag = true;
            this.nodeData = this.calcNode(1);
            // console.log(this.nodeData);
            [this.xAxis, this.yAxis] = this.calcAxis();
            this.tableData = this.calcTableData(this.sliceData);
        })
        // this.nodeData = this.calcNode(1);
        // // console.log(this.nodeData);
        // [this.xAxis, this.yAxis] = this.calcAxis();
        // this.tableData = this.calcTableData(this.sliceData);

        // console.log(this.tableData);
        // console.log(this.xAxis, this.yAxis)
    },
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
