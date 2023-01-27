<template>
  <div class="wrapper">
    <div class="angle-calculation">
    <div>
    <label for="">Години </label>
    <input type="number" v-model="data.h" />
  </div>
  <div>

    <label for="">Хвилини </label>
    <input type="number" v-model="data.m" />
  </div>
  <button @click="logResults()">Вирахувати</button>
  <span v-if="data.angle !== null">Кут: {{ data.angle }}&#176</span>
  </div>

  <div class="clock">
  <div id="hours" ref="hoursHand"></div>
  <div id="minutes" ref="minutesHand"></div>
  <div id="seconds" ref="secondsHand"></div>
  <div id="center"></div>
  </div></div>
</template> 

<script setup>
import { reactive, onMounted, onUpdated, ref } from "vue";


const data = reactive({ h: null, m: null, angle: null, nowHours: null, nowMinutes: null, nowSeconds: null });

const timeInterval = 6

function min(x, y) {
  return x < y ? x : y;
}

function calcAngle(h, m) {
  if (h < 0 || m < 0 || h > 12 || m > 60) {
    alert("Неправильні дані");
    return
  }

  if (h == 12) h = 0;
  if (m == 60) {
    m = 0;
    h += 1;
    if (h > 12) h = h - 12;
  }

  let hour_angle = 0.5 * (h * 60 + m);
  let minute_angle = 6 * m;

  let angle = Math.abs(hour_angle - minute_angle);

  angle = min(360 - angle, angle);

  data.angle = angle;

  return angle;
}

let logResults = () => {
  console.log(calcAngle(data.h, data.m));
};

const hoursHand = ref(null)
const minutesHand = ref(null)
const secondsHand = ref(null)


setInterval(function() {
  const now = new Date()
  const nowMinutes = now.getMinutes()
  const nowSeconds = now.getSeconds()
  const nowHours = now.getHours()

  hoursHand.value.style.transform = `translate(-50%, -100%) rotate(${nowHours * 30 + nowMinutes / 2}deg)`
  minutesHand.value.style.transform = `translate(-50%, -100%) rotate(${nowMinutes * timeInterval + nowSeconds / 10}deg)`
  secondsHand.value.style.transform = `translate(-50%, -100%) rotate(${nowSeconds * timeInterval}deg)`
}, 100)

</script>

<style scoped>

  .wrapper {
    position: relative;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .angle-calculation {
    background: lightcyan;
    padding: 10px;
    display: flex;
    justify-content: center;
    position: absolute;
    top: 0;
    left: 0;
  }
  button {
    display: block;
    margin-bottom: 20px;
  }

  .clock {
    height: clamp(246px, 636px, 30vw);
    width: clamp(246px, 636px, 30vw);
    border-radius: 50%;
    border: 3px solid green;
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  #seconds {
    height: 48%;
    width: 1px;
    background: blue;
    transform-origin: bottom;
    position: absolute;
    left: 50%;
    top: 50%;
  }

  #minutes {
    height: 40%;
    width: 3px;
    background: red;
    transform-origin: bottom;
    position: absolute;
    left: 50%;
    top: 50%;
  }

  #hours {
    height: 30%;
    width: 5px;
    background: yellow;
    transform-origin: bottom;
    position: absolute;
    left: 50%;
    top: 50%;
  }

  #center {
    height: 10px;
    width: 10px;
    border-radius: 50%;
    background: #000;
    position: absolute;
    z-index: 2;
  }

</style>
