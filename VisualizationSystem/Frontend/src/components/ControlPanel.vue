<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-03-20 02:00:52
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Profile View</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="ControlPanel" style="height: calc(30% - 10px); width: calc(100%); float: left; border: 0px solid blue; font-size: 16px;">
            <div style="height: calc(20% - 5px);">
                <span style="float: left; font-weight: normal; margin-top: 6px;">
                                DataSet:
                            </span>
                <span style="width: 60%; float: right;">
                                <el-select v-model="fileValue" class="m-2" placeholder="Select" size="large">
                                    <el-option v-for="item in fileOptions" :key="item.value" :label="item.label" :value="item.value" />
                                </el-select>
                            </span>
            </div>
            <div style="height: calc(20% - 5px); margin-top: 5px;">
                <span style="float: left; font-weight: normal; margin-top: 0px;">
                                Model:
                            </span>
                <span style="width: 60%; float: right; margin-top: 2px; text-align: end; margin-right: 10px;">
                                <!-- <el-select v-model="modelValue" class="m-2" placeholder="Select" size="large">
                                    <el-option v-for="item in modelOptions" :key="item.value" :label="item.label" :value="item.value" />
                                </el-select> -->
                                Long Short Term Memory
                            </span>
            </div>
            <div style="height: calc(20% - 5px); margin-top: 0px;">
                <span style="float: left; font-weight: normal; margin-top: 0px;">
                                Variable Num:
                            </span>
                <span style="width: 60%; float: right; margin-top: 2px; text-align: end; margin-right: 10px;">
                                <!-- <el-select v-model="modelValue" class="m-2" placeholder="Select" size="large">
                                    <el-option v-for="item in modelOptions" :key="item.value" :label="item.label" :value="item.value" />
                                </el-select> -->
                                6
                            </span>
            </div>
            <div style="height: calc(20% - 5px); margin-top: 0px;">
                <span style="float: left; font-weight: normal; margin-top: 0px;">
                                Stationary:
                            </span>
                <!-- <span style="width: 60%; float: right; font-weight: normal; margin-top: 6px; text-align: left;">
                                {{ fileValue == null ? '' : (basicData[fileValue].stationary['bool'] + ' (' +
                                    basicData[fileValue].stationary['p-value'] + ')') }}
                            </span> -->

                <span style="width: 60%; float: right; margin-top: 2px; text-align: end; margin-right: 10px;">
                                <!-- <el-select v-model="modelValue" class="m-2" placeholder="Select" size="large">
                                    <el-option v-for="item in modelOptions" :key="item.value" :label="item.label" :value="item.value" />
                                </el-select> -->
                                Yes
                            </span>
            </div>
            <div style="height: calc(20% - 5px); margin-top: 5px;">
                <span style="float: left; font-weight: normal; margin-top: 0px;">
                                Periodicity:
                            </span>
                <!-- <span style="width: 60%; float: right; font-weight: normal; margin-top: 6px; text-align: left;">
                                {{ fileValue == null ? '' : ((basicData[fileValue].periodicity['recommend_lag']) + ' (' +
                                    (Math.round(basicData[fileValue].periodicity['fft acf'] * 10000) / 10000) + ')') }}
                        </span> -->

                <span style="width: 60%; float: right; margin-top: 2px; text-align: end; margin-right: 10px;">
                                <!-- <el-select v-model="modelValue" class="m-2" placeholder="Select" size="large">
                                    <el-option v-for="item in modelOptions" :key="item.value" :label="item.label" :value="item.value" />
                                </el-select> -->
                                1, 3, 6, 13
                            </span>
            </div>
    
        </div>
        <div ref="resTable" style="height: calc(70% + 25px); width: calc(100%); float: right; overflow:auto; font-size: 18px; margin-top: -15px;">
            <el-table :data="tableData" style="width: calc(100% - 0px)" height="100%" :header-cell-style="{ 'font-size': '16px', 'background-color': 'rgb(235, 235, 235)', 'height': '40px', 'text-algin': 'center'}" :cell-style="{ 'font-size': '14px', 'height': '15px' }" :row-style="{ 'height': '18px' }" border>
                <el-table-column prop="smooth" label="Smooth" width="82" />
                <el-table-column prop="skip" label="Skip" width="62"/>
                <el-table-column label="Loss" :width="(elWidth - 142) / 3" sortable>
                    <template #default="scope">
                        <svg width="100%" height="18">
                            <rect :x="0" :y="3" :width="scope.row.train_bar.w" :height="15" :fill="'orange'" :fill-opacity="1"  :stroke="'rgb(200, 200, 200)'"> </rect>
                            <text x="2" y="15" font-size="12">{{ scope.row.train_bar.v }}</text>
                        </svg>
                    </template>
                </el-table-column>

                <el-table-column label="Val." :width="(elWidth - 142) / 3" sortable>
                    <template #default="scope">
                    
                        <svg width="100%" height="18">
                            <rect :x="0" :y="3" :width="scope.row.test_bar.w" :height="15" :fill="'orange'" :stroke="'rgb(200, 200, 200)'" :fill-opacity="1" ></rect>
                            <text x="2" y="15" font-size="12">{{ scope.row.test_bar.v }}</text>
                        </svg>
                    </template>
                </el-table-column>

                <el-table-column label="ACF" :width="(elWidth - 142) / 3" sortable>
                    <template #default="scope">
                    
                        <svg width="100%" height="18" >
                            <rect :x="0" :y="3" :width="scope.row.acf_bar.w" :height="15" :fill="'orange'"  :stroke="'rgb(200, 200, 200)'" :fill-opacity="1"></rect>
                            <text x="2" y="15" font-size="12">{{ scope.row.acf_bar.v }}</text>
                        </svg>
                    </template>
                </el-table-column>
                <!-- <el-table-column prop="address" label="Address" :formatter="formatter" /> -->
            </el-table>
        </div>
    </div>
</template>

<script>
import { scaleLinear } from 'd3-scale';
import multi_res_data from '../assets/allData/multivariate_data/model_results.json';
import uni_res_data from '../assets/allData/univariate_data/model_results.json';
import { useDataStore } from "../stores/counter";
export default {
    name: 'ControlPanelView',
    props: ['basicData'],
    data() {
        return {
            elHeight: 0,
            elWidth: 0,
            fileValue: null,
            tableData: [],
            fileOptions: [{
                    value: 'sunspots',
                    label: 'Sunspots',
                },

                {
                    value: 'pm',
                    label: 'PM 2.5',
                }
            ],
            modelValue: null,
            modelOptions: [{
                value: 'LSTM',
                label: 'LSTM',
            }],
            tableData: [],
        }
    },
    methods: {

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
        translate(x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        },
        calcTable(data) {
            // console.log(data);
            let tmpData = [];

            let max_train = 0,
                max_test = 0,
                max_acf = 0
            // for (let i = 0; i < 9; ++i) {
                for (let i in data) {
                for (const j in data[i].predic_info) {
                    let d = data[i].predic_info[j];
                    if (typeof(d['ACF']) == 'undefined') {
                        d['ACF'] = 0;
                    }
                    let tmp = new Object();
                    tmp['dataset_name'] = data[i].dataset_name;
                    let smooth_name = '';
                    if (data[i].dataset_name[1] == 'a') {
                        smooth_name = 'RAW';
                    } else {
                        if (data[i].dataset_name[1] == 'o') {
                            smooth_name = 'MA-';
                        } else if (data[i].dataset_name[1] == 'e') {
                            smooth_name = 'WMA-';
                        }
                        // console.log(typeof(data[i].dataset_name));
                        let stcnt = data[i].dataset_name;
                        let cnt = stcnt.substring(stcnt.length - 2);
                        if (!isNaN(Number(cnt))) {
                            smooth_name += cnt;
                        } else {
                            smooth_name += cnt[1];
                        }
                    }
                    tmp['smooth'] = smooth_name;
                    tmp['skip'] = d.skip;
                    // console.log(d.skip);
                    tmp['train'] = (d['train_MSE']);
                    tmp['test'] = (d['val_MSE']);
                    tmp['acf'] = (d['ACF']);
                    max_acf = Math.max(max_acf, parseFloat(data[i].predic_info[j]['ACF']));
                    max_train = Math.max(max_train, parseFloat(data[i].predic_info[j]['train_MSE']));
                    max_test = Math.max(max_test, parseFloat(data[i].predic_info[j]['val_MSE']));
                    tmpData.push(tmp);
                }
            }
            let leftT = 160;
            let barS = this.elWidth - leftT;
            let trainScale = scaleLinear([0, max_train], [0, ((barS) / 3) * 0.9]);
            let testScale = scaleLinear([0, max_test], [0, ((barS) / 3) * 0.9]);
            let acfScale = scaleLinear([0, max_acf], [0, ((barS) / 3) * 0.9]);
            console.log(tmpData);
            for (let i in tmpData) {
                tmpData[i]['train_bar'] = {
                    x: 0,
                    w: trainScale(tmpData[i]['train']),
                    v:( tmpData[i]['train'].toFixed(4)).toString().slice(1)
                };
                tmpData[i]['test_bar'] = {
                    x: barS / 3,
                    w: testScale(tmpData[i]['test']),
                    v: (tmpData[i]['test'].toFixed(4)).toString().slice(1)
                };
                tmpData[i]['acf_bar'] = {
                    x: barS * 2 / 3,
                    w: acfScale(tmpData[i]['acf']),
                    v: (tmpData[i]['acf'].toFixed(4)).toString().slice(1)
                };
            }
            // console.log(tmpData);
            return tmpData;
        }
    },
    watch: {
        fileValue() {
            // this.tableData = this.basicData[this.fileValue]['slice'];
            const dataStore = useDataStore();
            dataStore.dataSelect = this.fileValue;
            if (this.fileValue == 'sunspots') {
                this.tableData = this.calcTable(uni_res_data);
            } else if (this.fileValue == 'pm') {
                this.tableData = this.calcTable(multi_res_data);

            }

        },
        modelValue() {
            const dataStore = useDataStore();
            dataStore.model = this.modelValue;
        }
    },
    created() {},
    mounted() {
        this.elHeight = this.$refs.ControlPanel.offsetHeight;
        this.elWidth = this.$refs.ControlPanel.offsetWidth;
        // console.log(this.basicData)
        // this.tableData = this.calcTable(multi_res_data);
        // console.log(this.tableData);
    },
}
</script>

<style>
*,
*::before,
*::after {
    font-weight: bold;
}

.el-input__wrapper {
    height: 30px;
}

/* .el-table tr {
    height: 50px;
    font-size: 16px;
} */

/* .el-table__cell {
    height: 15px;
} */
.el-table .cell {
    padding: 0px;
    text-align: center;
}
/* td {
    height: 25%;
} */

/* .el-table .cell {
    padding-left: 3px;
    padding-right: 3px;
} */
</style>
