<template>
  <div id="app">
    <button type="button" @click="generate">
      {{ circle.length ? "Сбросить" : "Сгенерировать" }}
    </button>
    <button type="button" @click="setSort">
      Сортировка
    </button>

    <transition-group name="flip-list" tag="div" class="box">
      <div
        v-for="c in circle"
        :key="c.color"
        class="circle"
        :style="{
          backgroundColor: c.color
        }"
      ></div>
    </transition-group>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      circle: []
    }
  },

  computed: {},
  methods: {
    setSort() {
      this.circle
        .sort((a, b) =>
          this.optionSort(a.color) > this.optionSort(b.color) ? 1 : -1
        )
        .reverse()
    },
    optionSort(str) {
      let rgb = str
        .replace(/[rgb()]/g, "")
        .split(",")
        .map(Number)

      return rgb[0] + rgb[1] + rgb[2]
    },
    getRandomRGB(min, max) {
      return Math.floor(Math.random() * (max - min)) + min
    },
    // getRandomPositionY() {
    //   return Math.floor(Math.random() * 200)
    // },
    generate() {
      const setLength = Math.floor(Math.random() * 10) + 1
      if (!this.circle.length) {
        for (let i = 0; i < setLength; i++) {
          this.circle.push({
            color: `rgb(${this.getRandomRGB(0, 255)},${this.getRandomRGB(
              0,
              255
            )},${this.getRandomRGB(0, 255)})`
            // position: this.getRandomPositionY() + "%"
          })
        }
      } else {
        this.circle = []
      }
    }
  }
}
</script>

<style>
body {
  background-color: rgb(216, 216, 216);
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.box {
  display: flex;

  justify-content: space-around;
  align-items: center;

  width: 800px;
  height: 150px;
  background-color: #fff;
  margin: auto;
  transform: translateY(50%);
  position: relative;
}
.box .circle {
  width: 50px;
  height: 50px;
  display: inline-block;
  /* transform: translateY(200%) */
  border-radius: 50%;
}
.flip-list-move {
  transition: transform 1s;
}
</style>
