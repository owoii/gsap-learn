<script setup>
import { gsap } from 'gsap'
import { onMounted, ref } from 'vue'
import { Observer } from 'gsap/Observer'
import New from './components/svg/New.vue'

const scroll = ref()
const wind = ref()
const box1 = ref()
const speed = ref(1)
const speedM = 0.01
onMounted(() => {
  gsap.registerPlugin(Observer)
  speed.value =1
  Observer.create({
    target: window,
    type: 'wheel', //这里不监听页面滚动,而是监听滚轮
    onUp(e) {
      gsap.to(scroll.value, {
        y: '0%',
        x: 0,
        rotation:0
      })

      gsap.to(box1.value, {
        x: 0,
      })
      gsap.to(wind.value, {
        rotation: 0,
        x: 0,
      })
    },
    onDown(e) {
      speed.value += speedM
      gsap.to(scroll.value, {
        y: `-${(speed.value*2) + 100}%`,
        x: `-=${119 * speed.value}`,
        rotation:'+=0.05'
      })
      gsap.to(wind.value, {
        rotation: `+=${35*speed.value}`,
      })

      gsap.to(box1.value, {
        x: `+=${120 * speed.value}`,
      })
    },
  })
})
</script>

<template>
  <div class="container">
    <div class="scroll" ref="scroll">
      <div class="box box-blue">火车🚄</div>

      <!-- 第二页 -->
      <div class="box page2">
        <div class="text car" ref="box1">
          <div class="shoe">
            <div ref="wind" class="wind" v-for="item in 2">
              <New class="wind" />
            </div>
          </div>
        </div>
        <div class="gap-box">
          <div
            class="text g"
            v-for="iem in 100"
            :class="{
              'bg-blue': iem % 2 == 0,
            }"
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.container {
  overflow: hidden;
}
.box,
.container .scroll {
  height: 100vh;
  width: 100vw;
}
.box {
  padding: 18px 24px;
}
.box-blue {
  background: blue;
  background-image: url(https://img2.baidu.com/it/u=3548752401,2636297754&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=333);
  background-size: cover;
  color: transparent;
  background-clip: text;
  // -webkit-text-stroke: white 1px;
  display: flex;
  font-weight: 900;
  justify-content: center;
  align-items: center;
  font-size: 15rem;
}
.wind {
  height: 100px;
  width: 100px;
}
.text {
  margin-top: 50px;
  width: 500px;
  height: 200px;
  background: rgb(212, 30, 30) 000;
  flex-shrink: 0;
}
.gap-box {
  display: flex;
  gap: 15px;
}

.car {
  border-radius: 50px 999px 50px 50px;
  background-image: linear-gradient(135deg, #feb692 10%, #ea5455 100%);
  position: relative;
}
.shoe {
  position: absolute;
  bottom: -25%;
  width: 100%;
  display: flex;
  justify-content: space-between;
}
.page2 {
}
.g {
  background-image: linear-gradient(135deg, #fccf31 10%, #f55555 100%);
  border-radius: 99999px;
}
.bg-blue {
  background-image: linear-gradient(
    135deg,
    #ce9ffc 10%,
    #7367f0 100%
  ) !important;
}
</style>
<style>
body {
  background-image: linear-gradient(135deg, #fff6b7 10%, #f6416c 100%);
}
</style>
