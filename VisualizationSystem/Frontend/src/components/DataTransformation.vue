<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-01-10 21:25:50
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Data Transformation Overview</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="DataTransformation" style="height: 100%; width: 100%;">
            <svg id="DataTransformation" height="100%" width="100%">
                
            </svg>
        </div>
    </div>
</template>
<script>
export default {
    name: 'DataTransformationView',
    props: [],
    data() {
        return {
            elHeight: 0,
            elWidth: 0,
        }
    },
    methods: {
        translate(x, y, deg) {
            return `translate(${x}, ${y}) rotate(${deg})`;
        }
    },
    created() {
    },
    mounted() {
        this.elHeight = this.$refs.DataTransformation.offsetHeight;
        this.elWidth = this.$refs.DataTransformation.offsetWidth;
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
