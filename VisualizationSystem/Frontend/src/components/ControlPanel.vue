<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-01-10 21:28:34
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Control Panel</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="ControlPanel" style="height: 100%; width: 100%;">
            <svg id="ControlPanel" height="100%" width="100%">
                
            </svg>
        </div>
    </div>
</template>
<script>
export default {
    name: 'ControlPanelView',
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
        this.elHeight = this.$refs.ControlPanel.offsetHeight;
        this.elWidth = this.$refs.ControlPanel.offsetWidth;
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
