<!--
 * @Description: 
 * @Author: Qing Shi
 * @Date: 2022-09-17 23:36:36
 * @LastEditTime: 2023-01-12 04:11:35
-->
<template>
  <div class="common-layout" style="width: 100%; height: 100vh;" v-loading="initSign" :element-loading-text="loadingText"
        element-loading-background="rgba(0, 0, 0, 0.8)">
    <Main :msgH="msgH" :timeData="timeData"/>
  </div>
</template>

<script>
import Main from '../components/Main.vue';
import { useDataStore } from "../stores/counter";

import timeData from '../assets/sn_d_tot.json'
export default {
  name: "home_view",
  data() {
    return {
      msgH: null,
      timeData: null
    };
  },
  computed: {
    initSign() {
      this.timeData = timeData;
      // return this.msgH == null;
      return this.timeData == null;
    },
    loadingText() {
      return "Loading"
      // return "Loading:    " + 'Route: \n' +
      //     !!this.route + '   Station: \n'+ !!this.station + '   Alldata: \n'+ !!this.alldata
    }
  },
  mounted() {
    // const dataStore = useDataStore();
    // dataStore.fetchHello();
    // this.msgH = dataStore.msg;
      this.timeData = timeData;
    this.msgH = 1;
  },
  methods: {
    fetchData() {
    }
  },
  components: { Main }
};
</script>
<style scoped>

.boundary {
        /*border-style: dashed;*/
        border-style: solid;
        border-color: #d3dce6;
        border-width: 0.5px;
        border-radius: 3px;
    }
</style>
