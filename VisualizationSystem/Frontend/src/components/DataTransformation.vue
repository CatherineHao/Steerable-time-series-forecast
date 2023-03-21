<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-03-20 02:11:32
-->
<template>
    <div class="frameworkTitle" style="padding-right: 10px;">
        <div class="title">Temporal View</div>
        <p class="titleTriangle"></p>
        <div style="float: right; margin-top: 3px;">
            <span>Metric: </span>
            <el-select v-model="heatTag" class="m-2" placeholder="Select" style="width: 150px;">
                <el-option v-for="(item, i) in heatOptions" :key="item" :label="item" :value="i" />
            </el-select>
        </div>
    </div>
    <div class="frameworkBody">
    
        <div style="height: calc(100%); width: 100%; margin-top: 0px; overflow-y: auto;">
            <!-- <div style="height: 1000px;">
                
                            </div> -->
            <div style="height: 95%; overflow-y: auto; width: 100%;" ref="timeline">
                <div v-for="(item, i) in overview_line_data" :key="'overview_line_' + i">
                    <div :style="{
                        'height': (tlHeight / (overview_line_data.length * 3) - 3) + 'px', 'background-color': 'white',
                        'border': '1px solid black',
                        'margin-top': '3px'
                        }">
                        <svg width="100%" height="100%">
                            <g>
                                <path :d="item.d" :stroke="'none'" :fill="item.fill[0]" :transform="translate(0, -item.height * 3, 0)"></path>
                                <path :d="item.d" :stroke="'none'" :fill="item.fill[1]" :transform="translate(0, -item.height * 2, 0)"></path>
                                <path :d="item.d" :stroke="'none'" :fill="item.fill[2]"  :transform="translate(0, -item.height * 1, 0)"></path>
                                <path :d="item.d" :stroke="'none'" :fill="item.fill[3]"></path>
                                <text x="10" y="18">{{ item.feature }}</text>
                            </g>
                        </svg>
                    </div>
                </div>
            </div>
        </div>
    
    </div>
</template>

<script>
import { axisBottom, axisLeft } from 'd3-axis';
import { scaleLinear, scaleOrdinal } from 'd3-scale';
import { arc, area, line } from 'd3-shape';
import SN_raw_data from "../assets/SN_m_tot_V2.0.csv";
import multi_var_data from "../assets/15month_result/raw_15month.csv";
import { extent, max, sum } from 'd3-array';
import { brushX } from 'd3-brush';
import { select, selectAll } from 'd3-selection';
export default {
    name: 'DataTransformationView',
    props: ['timeData', 'sliceData'],
    data() {
        return {
            horizon_level: 4,
            elHeight: 1000,
            elWidth: 1000,
            tlHeight: 100,
            tlWidth: 100,
            featureNum: [],
            showTag: '',
            heatHeight: 0,
            heatTag: 3,
            timeBrush: null,
            allTimeScale: null,
            heatOptions: ['Raw + Difference', 'Raw', 'Difference', 'RMSE + Corr.', 'RMSE', 'Corr.'],
            sample: ['10-slice', '7-slice', '3-slice'],
            smooth: ['Raw Sequence', 'N-Average', 'EMA/Holt'],
            d: [],
            startTime: 0,
            timeScale: null,
            maeScale: null,
            timeSliceData: [],
            barData: [],
            tableData: [],
            rawTimeLineData: 'M 0 0',
            brushTimeLineData: 'M 0 0',
            smoothTimeLineData: 'M 0 0',
            timeAxis: [],
            brushTimeAxis: [],
            sparkboxData: [],
            heatRectData: [],
            selectAverageLine: [],
            scaleRange: {
                raw: [99999, -999999],
                error: [99999, -999999]
            },
            groupPath: [],
            smoothData: [],
            select_smooth_data: [],
            select_time_step: [0, 2459],
            lineGenerateFunc: null,
            yScale: null,
            xScale: null,
            rxScale: null,
            lineData: [],
            smoothLineData: [],
            skip_length: [13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6, 13, 1, 3, 6],
            smoothTag: 0,
            timeLinePath: null,
            linePathTag: 0,
            coverRect: [],
            predict_line: [],
            predict_tag: 0,
            overview_line_data: [],
            horizon_color: ['#c7dbee','#a1cadf', '#4892c3', '#0e4591']
        }
    },
    methods: {
        translate(x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        },
        calcSparkBox(data, height, width) {
            let margin = ({ top: 20, right: 20, bottom: 30, left: 50 });
            // let height = 440;
            // let width = 1000;
            let focusHeight = 100;

            let y = scaleLinear()
                .domain([0, max(data, d => parseFloat(d.value))])
                .range([height - focusHeight - 30 - margin.bottom, margin.top])
            let sBData = [];
            let y2 = scaleLinear()
                .domain([0, max(data, d => parseFloat(d.value))])
                .range([focusHeight, margin.top])
            // let x = scaleLinear()
            //     .domain([0, max(data, d => parseInt(d.id))])
            //     .range([margin.left, width - margin.right])
            let x = this.xScale;
            let sparkBoxData = [];
            let timeGap = 130
            for (let i = 0; i < data.length; i += timeGap) {
                // let tempValue = Array.from(new Set(data.slice(i, i + 10).map(d => parseFloat(d.value)).sort((a, b) => a - b)));
                let tempValue = data.slice(i, i + timeGap).map(d => parseFloat(d.value)).sort((a, b) => a - b)
                let sumData = sum(tempValue);

                // console.log(sumData)

                // console.log(tempValue[tempValue.length /2 - 1], tempValue.length /2 - 1);
                sBData.push({
                    x: x(parseInt(data[i].id)),
                    y: y(tempValue[tempValue.length - 1]),
                    w: Math.abs(x(parseInt(data[i + ((i + timeGap < data.length) ? timeGap : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                    h: Math.abs(y(tempValue[0]) - y(tempValue[tempValue.length - 1]))
                })
                sBData.push({
                    x: x(parseInt(data[i].id)),
                    y: y(tempValue[parseInt(tempValue.length * 3 / 4) - 1]),
                    w: Math.abs(x(parseInt(data[i + ((i + timeGap < data.length) ? timeGap : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                    h: Math.abs(y(tempValue[parseInt(tempValue.length * 3 / 4) - 1]) - y(tempValue[parseInt(tempValue.length / 4) - 1]))
                })
                sBData.push({
                    x: x(parseInt(data[i].id)),
                    // y: y(tempValue[parseInt(tempValue.length /2) - 1]),
                    // y: y((tempValue[0] + tempValue[parseInt(tempValue.length - 1)]) / 2),
                    y: y(sumData / tempValue.length),
                    w: Math.abs(x(parseInt(data[i + ((i + timeGap < data.length) ? timeGap : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                    h: 1
                })
                sparkBoxData.push({
                    rect1: {
                        x: x(parseInt(data[i].id)),
                        y: y(tempValue[tempValue.length - 1]),
                        w: Math.abs(x(parseInt(data[i + ((i + 10 < data.length) ? 10 : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                        h: Math.abs(y(tempValue[0]) - y(tempValue[tempValue.length - 1]))
                    },
                    rect2: {
                        x: x(parseInt(data[i].id)),
                        y: y(tempValue[parseInt(tempValue.length * 3 / 4) - 1]),
                        w: Math.abs(x(parseInt(data[i + ((i + 10 < data.length) ? 10 : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))),
                        h: Math.abs(y(tempValue[parseInt(tempValue.length * 3 / 4) - 1]) - y(tempValue[parseInt(tempValue.length / 4) - 1]))
                    },
                    line: {
                        x1: x(parseInt(data[i].id)),
                        // y: y(tempValue[parseInt(tempValue.length /2) - 1]),
                        // y: y((tempValue[0] + tempValue[parseInt(tempValue.length - 1)]) / 2),
                        y: y(sumData / tempValue.length),
                        x2: Math.abs(x(parseInt(data[i + ((i + 10 < data.length) ? 10 : (data.length - 1 - i))].id)) - x(parseInt(data[i].id))) + x(parseInt(data[i].id))
                    }
                })
            }
            selectAll('.sparkbox').remove();
            selectAll('#time_path_raw').remove();
            select('#brush_path_g').append('g').attr('class', 'sparkbox').selectAll('#sparkRect').attr('id', 'sparkRect').data(sBData).enter().append('rect').attr('x', d => d.x).attr('y', d => d.y).attr('width', d => d.w).attr('height', d => d.h).attr('fill', (d, i) => {
                if (i % 3 == 0) {
                    return '#f2f5fa';
                } else if (i % 3 == 1) {
                    return '#dce3f3'
                } else return '#6d70b6';
            });
            select('#brush_path_g')
                // .append('g')
                // .data([{
                //     value: data
                // }])
                // .enter()
                .append('path')
                .attr('id', 'time_path_raw')
                .attr('d', d => {
                    return this.timeLinePath
                })
                .attr('stroke', '#777')
                .attr('stroke-width', 1.5)
                .attr('fill', 'none')
                .style('z-index', 5)

            // console.log(sparkboxData)
            // return sparkboxData;
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
            this.timeBrush = timeBrush;
            let x = scaleLinear()
                .domain([0, max(data, d => parseInt(d.id))])
                .range([margin.left, width - margin.right])
            this.allTimeScale = x;

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
                select('#focusLine_g').append('g').attr('id', 'axsg').attr("transform", `translate(0, ${-30})`).call(axisBottom(tScale));

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
                _this.xScale.domain(timeStep);
                // _this.yScale.domain([0, maxY]);
                let xAxis = [];
                let timeRange = [];
                let lenRange = [];
                let cnt_i = 0
                // select("#timeline_g").append('path').attr('id', 'selected_area').attr('d', 'M ' + parseInt(988) + ' ' + 85 + ' L ' + parseInt(1760) + ' ' + 85 + ' L ' + (_this.elWidth) + ' 130 L 50 130 Z').attr('stroke', '#777').attr('stroke-width', 1).attr('fill', '#777').attr('opacity', 0.15);    
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
                if (_this.predict_tag) {

                    select('#sel_predict_line').attr('d', lineGenerate(_this.predict_line));
                }

                select("#selected_area").attr('d', 'M ' + parseInt(selection[0]) + ' ' + 441 + ' L ' + parseInt(selection[1]) + ' ' + 441 + ' L ' + (_this.tlWidth - margin.right) + ' 393 L 50 393 Z')
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
        calcTimeLine(data, height, width) {
            let margin = ({ top: 20, right: 20, bottom: 30, left: 50 });
            let focusHeight = 100;

            let y = scaleLinear()
                .domain([0, max(data, d => parseFloat(d.value))])
                .range([height - focusHeight - 30 - margin.bottom, margin.top])
            let y2 = scaleLinear()
                .domain([0, max(data, d => parseFloat(d.value))])
                .range([focusHeight, margin.top])
            let x = scaleLinear()
                .domain([0, max(data, d => parseInt(d.id))])
                .range([margin.left, width - margin.right])
            const rx = scaleLinear()
                .domain([margin.left, width - margin.right])
                .range([0, max(data, d => parseInt(d.id))]);

            let timeData = [];
            let lenData = [];
            for (let i = 0; i < data.length; ++i) {

                if (i == 0 || i == data.length - 1 || i % Math.floor(data.length / 10) === 0) {
                    lenData.push(x(parseInt(data[i].id)));
                    timeData.push(parseInt(data[i].timestamp));
                }
            }
            let timeScale = scaleOrdinal(timeData, lenData);
            select('#focusLine_g').append('g').call(axisBottom(timeScale).tickSizeOuter(0).tickPadding(10)).attr('transform', `translate(0,${focusHeight})`).call(g => g.selectAll(".title").data(['Time']).join("text")
                .attr("class", "title")
                .attr("x", this.elWidth - 20)
                .attr("y", 35)
                .attr("fill", "currentColor")
                .attr("text-anchor", "middle")
                .attr('font-size', '14px')
                .text(''))
            this.xScale = x;
            this.yScale = y;
            this.rxScale = rx;
            // console.log(y.domain(), x.domain(), data);

            // let area = (x, y) => d3.area()
            //     .defined(d => !isNaN(d.value))
            //     .x(d => x(d.date))
            //     .y0(y(0))
            //     .y1(d => y(d.value))
            let lineGenerate = line()
                .x(d => x(d.id))
                .y(d => y(d.value));
            this.lineGenerateFunc = lineGenerate;

            let lineGenerate2 = line()
                .x(d => x(d.id))
                .y(d => y2(d.value));

            let yAxis = (g, y, title) => g
                .attr("transform", `translate(${margin.left},${0})`)
                .call(axisLeft(y))
                // .call(g => g.select(".domain").remove())
                .call(g => g.selectAll(".title").data([title]).join("text")
                    .attr("class", "title")
                    .attr("x", 0)
                    .attr("y", 12)
                    .attr("fill", "currentColor")
                    .attr("text-anchor", "middle")
                    .attr('font-size', '14px')
                    .text(title));
            let xAxis = (g, x, height) => g
                .attr("transform", `translate(0,${height - margin.bottom})`)
                .call(axisBottom(x).ticks(width / 80).tickSizeOuter(0));
            let timeAxis = []
            for (let i = 0; i < data.length; i += parseInt(data.length / 10)) {
                timeAxis.push({
                    x: x(i),
                    text: parseInt(parseInt(data[i].timestamp) / 100) + '.' + parseInt(parseInt(data[i].timestamp) % 100)
                })
            }
            this.timeAxis = timeAxis;
            this.brushTimeAxis = timeAxis;
            select('#raw_line_g').append('g').attr('id', 'yAxis_g').call(yAxis, y, 'Value');
            this.timeLinePath = lineGenerate(data);
            const _this = this;



            // select('#brush_path_line')
            //     .append('path')
            //     .attr('id', 'time_brush')
            //     .attr('d', d => {
            //         return lineGenerate2(data)
            //     })
            //     .attr('stroke', '#777')
            //     .attr('stroke-width', 1)
            //     .attr('fill', 'none')
            this.lineData = data;

            // return [lineGenerate(data), lineGenerate2(data)];
        },
        calcMonth(startTime, endTime) {
            let year = parseInt(endTime / 100) - parseInt(startTime / 100);
            let month = endTime % 100 - startTime % 100 + 1;
            let sumMonth = year * 12 + month;
            return sumMonth;
        },
        calcFeatureLine(data, width, height) {
            let margin = { left: 0, right: 0, top: 0, bottom: 0 };
            let xScale = scaleLinear([0, data.length - 1], [margin.left, width - margin.right]);
            let vRange = extent(data, d => parseFloat(d.value));
            let yScale = scaleLinear(vRange, [height - margin.bottom, margin.top]);
            let lineGenerate = line().x((d, i) => xScale(i)).y(d => yScale(parseFloat(d.value)));
            let line_data = lineGenerate(data);
            return line_data;
        },
        calcFeatureArea(data, width, height) {
            let margin = { left: 0, right: 0, top: 0, bottom: 0 };
            
            let xScale = scaleLinear([0, data.length - 1], [margin.left, width - margin.right]);
            let vRange = extent(data, d => parseFloat(d.value));
            // vRange
            let yScale = scaleLinear(vRange, [this.horizon_level * (height - margin.bottom), margin.top]);
            let areaGenerate = area().x((d, i) => xScale(parseFloat(i))).y0(d => yScale(parseFloat(d.value))).y1(d => yScale(parseFloat(vRange[0])));
            let area_data = areaGenerate(data);
            return area_data;
        },
        calcOverviewTimeLine(data) {
            console.log(data);
            let featureSet = [];
            let allData = {};
            for (let i in data[0]) {
                if (i == 'date')
                    continue;
                featureSet.push(i);
                allData[i] = [];
            }
            // featureSet = ['raw']
            // allData['raw'] = []
            for (let i in data) {
                for (const j of featureSet) {
                    // console.log(data[i][j])
                    allData[j].push({
                        date: data[i]['id'],
                        value: parseFloat(data[i][j])
                    });
                }
            }
            console.log(allData);
            let result_data = new Array();
            // for (let i in allData) {
            //     result_data.push({
            //         d: this.calcFeatureLine(allData[i], this.tlWidth, this.tlHeight / (featureSet.length *2)),
            //         raw: allData[i],
            //         feature: i
            //     })
            // }

            for (let i in allData) {
                result_data.push({
                    d: this.calcFeatureArea(allData[i], this.tlWidth, this.tlHeight / (featureSet.length *2)),
                    raw: allData[i],
                    feature: i,
                    fill: this.horizon_color,
                    height: this.tlHeight / (featureSet.length *2)
                })
            }
            // console.log(result_data);
            return result_data;
        }
    },
    created() {},
    mounted() {
        this.tlHeight = this.$refs.timeline.offsetHeight * 1;
        this.tlWidth = this.$refs.timeline.offsetWidth;
        // console.log(SN_raw_data)


        this.overview_line_data = this.calcOverviewTimeLine(multi_var_data);

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
</style>
