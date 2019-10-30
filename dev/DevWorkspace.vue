<template>
  <div class='dev-workspace-container'>
    <ios13-segmented-control v-model="value" :segments="segments"/>
  </div>
</template>

<script>

import Vue from 'vue';
import iOS13SegmentedControl from "@/vue-ios-13-segmented-control.vue";

Vue.component("ios13-segmented-control", iOS13SegmentedControl);

export default {
  name: 'DevWorkspace',
  data() {
    return {
      value: "42",
      segments: [
        {
          title: "Apple Music",
          id: "42"
        },
        {
          title: "Spotify",
          id: "93"
        },
        {
          title: "Deezer",
          id: "11"
        },
        {
          title: "Tidal",
          id: "1"
        }
      ]
    };
  },
  
};
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style lang='scss'>

.dev-workspace-container {
  margin: 0px;
}
</style>