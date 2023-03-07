<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-02-02 15:11:12
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Control Panel</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="ControlPanel"
            style="height: 30%; width: calc(100%); float: left; border: 0px solid blue; font-size: 16px;">
            <!-- <svg id="ControlPanel" height="100%" width="100%">
                
            </svg> -->
            <div style="height: calc(20% - 5px);">
                <span style="float: left; font-weight: normal; margin-top: 6px;">
                    DataSet:
                </span>
                <span style="width: 60%; float: right;">
                    <el-select v-model="fileValue" class="m-2" placeholder="Select" size="large">
                        <el-option v-for="item in fileOptions" :key="item.value" :label="item.label"
                            :value="item.value" />
                    </el-select>
                </span>
            </div>
            <div style="height: calc(20% - 5px); margin-top: 5px;">
                <span style="float: left; font-weight: normal; margin-top: 6px;">
                    Model:
                </span>
                <span style="width: 60%; float: right;">
                    <el-select v-model="modelValue" class="m-2" placeholder="Select" size="large">
                        <el-option v-for="item in modelOptions" :key="item.value" :label="item.label"
                            :value="item.value" />
                    </el-select>
                </span>
            </div>
            <div style="height: calc(20% - 5px); margin-top: 5px;">
                <span style="float: left; font-weight: normal; margin-top: 6px;">
                    Multi-variable:
                </span>
                <span style="width: 60%; float: right;">
                    
                </span>
            </div>
            <div style="height: calc(20% - 5px); margin-top: 5px;">
                <span style="float: left; font-weight: normal; margin-top: 6px;">
                    Stationary:
                </span>
                <span style="width: 60%; float: right; font-weight: normal; margin-top: 6px; text-align: left;">
                    {{ fileValue == null ? '' : (basicData[fileValue].stationary['bool'] + ' (' +
                    basicData[fileValue].stationary['p-value'] + ')') }}
                </span>
            </div>
            <div style="height: calc(20% - 5px); margin-top: 5px;">
                <span style="float: left; font-weight: normal; margin-top: 6px;">
                    Periodicity:
                </span>
                <span style="width: 60%; float: right; font-weight: normal; margin-top: 6px; text-align: left;">
                    {{ fileValue == null ? '' : ((basicData[fileValue].periodicity['recommend_lag']) + ' (' +
                    (Math.round(basicData[fileValue].periodicity['fft acf'] * 10000) / 10000) + ')') }}
                </span>
            </div>

        </div>
        <div ref="resTable" style="height: 70%; width: calc(100%); float: right; overflow:auto; font-size: 18px;">
            <el-table :data="tableData" style="width: 100%" height="100%"
                :header-cell-style="{ 'text-align': 'center', 'font-size': '16px', 'background-color': 'rgba(250, 250, 250, 1)' }"
                :cell-style="{ 'text-align': 'center', 'font-size': '16px', 'height': '15px', 'padding-left': '5px', 'padding-right': '5px'}"
                :row-style="{ 'height': '18px' }">
                <!-- <el-table-column label="ID" prop="id" sortable /> -->
                <el-table-column label="Smooth" prop="dataset_name" width="90"/>
                <el-table-column label="Skip" prop="skip" width="60" />
                <el-table-column label="Loss" prop="test" width="60"  />
                <el-table-column label="ACF" prop="acf" width="60" />
            </el-table>
        </div>
        <!-- <div ref="ControlTable" style="height: 100%; width: calc(70% - 7.5px); float: right; background-color: green;">
            <el-table :data="tableData" stripe border style="width: 100%; height: 100%;" :header-cell-style="{'text-align':'center', 'background-color': 'rgb(250, 250, 250)'}" :cell-style="{'text-align':'center'}">
                <el-table-column prop="slice_number" label="Slice number" width="120" />
                <el-table-column prop="train length" label="Train length" width="120" />
                <el-table-column prop="skip length" label="Skip length" width="120" />
                <el-table-column prop="remaining data" label="Remaining data" width="140" />
                <el-table-column prop="test length" label="Test length" />
            </el-table>

        </div> -->
    </div>
</template>
<script>
import res_data from '../assets/model_skip_results.json';
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
            fileOptions: [
                {
                    value: '13_average_smooth_sunspot',
                    label: '13_average_smooth_sunspot',
                },

                {
                    value: 'raw_sunspot',
                    label: 'raw_sunspot',
                }
            ],
            modelValue: null,
            modelOptions: [
                {
                    value: 'LSTM',
                    label: 'LSTM',
                }
            ],
            tableData: [],
            tableDataB: [
                {
                    date: '2016-05-03',
                    name: 'Tom',
                    address: 'No. 189, Grove St, Los Angeles',
                },
                {
                    date: '2016-05-02',
                    name: 'Tom',
                    address: 'No. 189, Grove St, Los Angeles',
                },
                {
                    date: '2016-05-04',
                    name: 'Tom',
                    address: 'No. 189, Grove St, Los Angeles',
                }
            ]
        }
    },
    methods: {
        
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
        translate(x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        },
        calcTable(data) {
            // console.log(data);
            let tmpData = [];
            for (let i = 0; i < 9; ++i) {
                for (const j in data[i].predic_info) {
                    let d = data[i].predic_info[j];
                    let tmp = new Object();
                    tmp['dataset_name'] = data[i].dataset_name;
                    tmp['skip'] = d.skip;
                    // console.log(d.skip);
                    tmp['train'] = this.formatNum(d['loss=mean_squared_error']);
                    tmp['test'] = this.formatNum(d['val_loss=val_mse']);
                    tmp['acf'] = this.formatNum(d['ACF']);
                    tmpData.push(tmp);
                }
            }
            // console.log(tmpData);
            return tmpData;
        }
    },
    watch: {
        fileValue () {
            this.tableData = this.basicData[this.fileValue]['slice'];
            
        },
        modelValue() {
            const dataStore = useDataStore();
            dataStore.model = this.modelValue;
        }
    },
    created() {
    },
    mounted() {
        this.elHeight = this.$refs.ControlPanel.offsetHeight;
        this.elWidth = this.$refs.ControlPanel.offsetWidth;
        // console.log(this.basicData)
        this.tableData = this.calcTable(res_data);
    },
}
</script>
<style>
*,
*::before,
*::after {
    font-weight: bold;
}
.el-table tr {
    height:50px;
    font-size: 16px;
}
td {
    height:25%;
}
.el-table .cell {
    padding-left: 3px;
    padding-right: 3px;
}
</style>
