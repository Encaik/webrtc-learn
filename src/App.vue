<template>
  <WebRTC></WebRTC>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import WebRTC from "./components/webrtc.vue";

@Options({
  components: {
    WebRTC,
  },
})
export default class App extends Vue { }
</script>

<style lang="less">
* {
  padding: 0;
  margin: 0;
}
#app {
  height: 100vh;
  width: 100vw;
}
</style>
