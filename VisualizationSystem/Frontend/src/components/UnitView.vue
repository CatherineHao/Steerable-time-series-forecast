<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2023-01-10 21:20:01
 * @LastEditTime: 2023-01-10 22:18:32
-->
<template>
    <div class="frameworkTitle">
        <div class="title">Unit View</div>
        <p class="titleTriangle"></p>
    </div>
    <div class="frameworkBody">
        <div ref="UnitView" style="height: 100%; width: 100%;">
            <img src="../assets/img/a.png" alt="" style="height: 100%;">
        </div>
    </div>
</template>
<script>
export default {
    name: 'UnitView',
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
        this.elHeight = this.$refs.UnitView.offsetHeight;
        this.elWidth = this.$refs.UnitView.offsetWidth;
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
