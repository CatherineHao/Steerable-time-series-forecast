<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-01-10 21:27:18
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Correlation View</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="CorrelationView" style="height: 100%; width: 100%;">
            <svg id="CorrelationView" height="100%" width="100%">
                
            </svg>
        </div>
    </div>
</template>
<script>
export default {
    name: 'CorrelationView',
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
        this.elHeight = this.$refs.CorrelationView.offsetHeight;
        this.elWidth = this.$refs.CorrelationView.offsetWidth;
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
