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
            style="height: 100%; width: calc(30% - 7.5px); float: left; border: 0px solid blue; font-size: 18px;">
            <!-- <svg id="ControlPanel" height="100%" width="100%">
                
            </svg> -->
            <div style="height: 25%;">
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
            <div style="height: 25%;">
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
            <div style="height: 25%;">
                <span style="float: left; font-weight: normal; margin-top: 6px;">
                    Stationary:
                </span>
                <span style="width: 60%; float: right; font-weight: normal; margin-top: 6px; text-align: left;">
                    {{ fileValue == null ? '' : (basicData[fileValue].stationary['bool'] + ' (' +
                    basicData[fileValue].stationary['p-value'] + ')') }}
                </span>
            </div>
            <div style="height: 25%;">
                <span style="float: left; font-weight: normal; margin-top: 6px;">
                    Periodicity:
                </span>
                <span style="width: 60%; float: right; font-weight: normal; margin-top: 6px; text-align: left;">
                    {{ fileValue == null ? '' : ((basicData[fileValue].periodicity['recommend_lag']) + ' (' +
                    (Math.round(basicData[fileValue].periodicity['fft acf'] * 10000) / 10000) + ')') }}
                </span>
            </div>

        </div>
        <div ref="ControlTable" style="height: 100%; width: calc(70% - 7.5px); float: right; background-color: green;">
            <!-- <svg id="ControlPanel" height="100%" width="100%">
                
            </svg> -->
            <el-table :data="tableData" stripe border style="width: 100%; height: 100%;" :header-cell-style="{'text-align':'center', 'background-color': 'rgb(250, 250, 250)'}" :cell-style="{'text-align':'center'}">
                <el-table-column prop="slice_number" label="Slice number" width="120" />
                <el-table-column prop="train length" label="Train length" width="120" />
                <el-table-column prop="skip length" label="Skip length" width="120" />
                <el-table-column prop="remaining data" label="Remaining data" width="140" />
                <el-table-column prop="test length" label="Test length" />
            </el-table>

        </div>
    </div>
</template>
<script>
import { useDataStore } from "../stores/counter";
export default {
    name: 'ControlPanelView',
    props: ['basicData'],
    data() {
        return {
            elHeight: 0,
            elWidth: 0,
            fileValue: null,
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
        translate(x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
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
</style>
