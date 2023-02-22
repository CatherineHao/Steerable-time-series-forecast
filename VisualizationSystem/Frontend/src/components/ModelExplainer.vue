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
        <!-- <div ref="modelTable"
                                                            style="height: 100%; width: calc(50% - 7.5px + 30px); float: left; overflow:auto; font-size: 18px;">
                                                            <el-table :data="tableData" style="width: 100%" height="100%"
                                                                :header-cell-style="{ 'text-align': 'center', 'font-size': '16px', 'background-color': 'rgba(250, 250, 250, 1)' }"
                                                                :cell-style="{ 'text-align': 'center', 'background-color': 'rgba(250, 250, 250, 1)', 'font-size': '16px' }">
                                                                <el-table-column type="expand">
                                                                    <template #default="props">
                                                                        <div m="4">
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
                                                        </div> -->
        <div ref="modelExplainer" style="height: 100%; width: calc(100%); float: left;">
            <svg id="modelExplainer" height="100%" width="100%">

                
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
export default {
    name: 'modelExplainerView',
    props: ['sliceData'],
    data () {
        return {
            elHeight: 0,
            elWidth: 0,
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

        }
    },
    created () {
    },
    mounted () {
        this.elHeight = this.$refs.modelExplainer.offsetHeight;
        this.elWidth = this.$refs.modelExplainer.offsetWidth;
        
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
