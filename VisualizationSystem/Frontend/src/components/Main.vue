<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2022-11-20 23:25:35
 * @LastEditTime: 2023-02-18 20:11:49
-->
<template>
    <div id="navBar">DataTransAnalyzer</div>
    <div style="height: calc(94vh - 0px); width: calc(100% - 0px);">
        <div class="framework" id="controlPanel"
            style="position: absolute; left: calc(10px); top: calc(10px); height: calc(27vh - 20px); width: calc(12vw - 17.5px);">
            <!-- <ReasoningView /> -->
            <ControlPanel :basicData="basicData"/>
        </div>
        <div class="framework" id="DataTransformation"
            style="position: absolute; left: calc(10px + 40vw - 17.5px + 15px); top: calc(10px); height: calc(94vh - 20px); width: calc(60vw - 17.5px);">
            <DataTransformation :timeData="timeData" :sliceData="sliceData"/>
        </div>
        <div class="framework" id="ModelExplainer"
            style="position: absolute; left: calc(10px); top: calc(54vh + 5px); height: calc(40vh - 15px); width: calc(40vw - 17.5px);">
            <ModelExplainer :sliceData="sliceData"/>
        </div>
        <div class="framework" id="CorrelationView"
            style="position: absolute; left: calc(10px + 12vw);  top: calc(10px); height: calc(27vh - 20px); width: calc(3vw + (50vw - 17.5px) / 2 - 7.5px);">
            <CorrelationView />
        </div>
        <div class="framework" id="DistributionView"
            style="position: absolute; left: calc(10px);  top: calc(5px + 27vh); height: calc(27vh - 15px); width: calc(40vw - 17.5px);">
            <!-- <CorrelationView /> -->
        </div>
        <!-- <div class="framework" id="UnitView"
            style="position: absolute; left: calc(10px + 3vw + (50vw - 17.5px) / 2 + 7.5px);  top: calc(27vh + 5px); height: calc(27vh - 20px); width: calc((50vw - 17.5px) / 2 - 3vw - 7.5px);">
        <UnitView/>
        </div> -->
    </div>
</template>
<script>
import { dataService } from '@/service'
import CorrelationView from './CorrelationView.vue';
import ModelExplainer from './ModelExplainer.vue';
import DataTransformation from './DataTransformation.vue';
import ControlPanel from './ControlPanel.vue';
import UnitView from './UnitView.vue';
export default {
    name: "APP",
    props: ["msgH", 'timeData', 'sliceData', 'basicData'],
    data() {
        return {
            msg1: "Hello, main!"
        };
    },
    methods: {},
    created() {
    },
    mounted() {
    },
    components: { CorrelationView, ModelExplainer, DataTransformation, ControlPanel, UnitView }
}
</script>
<style>
.framework {
    /* margin: 5px; */
    border: 1px solid rgb(105, 119, 122);

    /* border-color: rgb(237, 237, 238); */
    /* border-color: ; */

    /* -webkit-box-shadow: 0px 3px 3px 2px #bdbaba;
    -moz-box-shadow: 0px 3px 3px 2px #bdbaba;
    box-shadow: 0px 3px 3px 2px #bdbaba; */

    /* height: calc(50% - 30px);
    width: calc(50% - 40px); */
    /* margin-top: 10px; */
    /* margin-left: 30px; */
}

.framework .frameworkTitle {
    /* background-color: rgb(151, 196, 208); */
    height: 50px;
    /* width: 100%; */
}

.framework .frameworkBody {
    /* background-color: #000; */
    height: calc(100% - 60px);
    /* width: 100%; */
    /* background-color: #4C87EC; */
    padding: 0px 10px 10px 10px;
}

.framework .frameworkTitle .title {
    /* position: absolute; */
    /* border-radius: 5px; */
    float: left;
    width: fit-content;

    background-color: rgb(105, 119, 122);
    /* margin-top: 7px;
    margin-left: 10px; */
    height: 40px;
    /* widows: auto; */

    font-weight: bold;
    font-size: 24px;
    color: white;
    padding-left: 10px;
    padding-right: 3px;
    font-family: 'KoHo';
    line-height: 35px;
}

.framework .frameworkTitle .titleTriangle {
    /* position: absolute;
    left: 180px; */
    /* display: inline; */
    float: left;
    width: 0;
    height: 0;
    border-color: transparent rgb(105, 119, 122);
    border-width: 0px 0px 40px 35px;
    border-style: solid;
}

#navBar {
    background-color: rgba(83, 79, 79);
    /* background-color: rgba(0, 0, 0, 0.8); */
    /* padding-left: 36%; */
    /* text-algin: center; */
    text-align: left;
    font-weight: bold;
    color: white;
    font-size: 28px;
    height: calc(6%);
    padding-top: 0.5%;
    padding-bottom: 0.5%;
    padding-left: 20px;
    /* font-family: STHeiti; */
    font-family: 'KoHo', 'Avenir', Helvetica, Arial, sans-serif;
}
</style>
