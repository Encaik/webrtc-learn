<template>
  <canvas id="wave"></canvas>
</template>

<script setup>
import { onMounted } from "vue";
onMounted(() => {
  let canvasEl = document.getElementById("wave");
  canvasEl.width = window.innerWidth;
  canvasEl.height = window.innerHeight;
  let canvasCtx = canvasEl.getContext("2d");
  canvasCtx.strokeStyle = "red";
  navigator.mediaDevices.getUserMedia({ audio: true }).then((stream) => {
    let audioCtx = new window.AudioContext();
    let analyser = audioCtx.createAnalyser();
    analyser.fftSize = 512;
    let source = audioCtx.createMediaStreamSource(stream);
    source.connect(analyser);
    analyser.connect(audioCtx.destination);
    let frequencyDataArr = new Uint8Array(analyser.frequencyBinCount);
    let wUnit = window.innerWidth / 256;
    let hUnit = window.innerHeight / 256;
    draw();

    function draw() {
      requestAnimationFrame(draw);
      analyser.getByteFrequencyData(frequencyDataArr);
      console.log(frequencyDataArr);
      canvasCtx.beginPath();
      canvasCtx.clearRect(0, 0, window.innerWidth, window.innerHeight);
      canvasCtx.moveTo(0 * wUnit, 256 * hUnit);
      frequencyDataArr.forEach((item, index) => {
        canvasCtx.lineTo(index * wUnit, item * hUnit);
        canvasCtx.stroke();
      });
    }
  });
});
</script>

<style scoped lang="less"></style>
