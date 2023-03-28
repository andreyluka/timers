<script>
import timer from './components/timer.vue'

export default {
  components: {
    timer
  },
  data() {
    return {
      timers: [
        {
          "id": 1,
          "time": 0,
          "active": false,
          "interval": null
        },
        {
          "id": 2,
          "time": 0,
          "active": false,
          "interval": null
        },
      ]
    }
  },
  methods: {
    addTimer() {
      this.timers.push({
        id: 1 + Math.max(0, ...this.timers.map(n => n.id)),
        time: 0,
        active: false,
        interval: null
      });
    },
    startTimer(id) {
      this.timers.forEach(el => {
        if (el.id === id) {
          el.active = !el.active

          if (el.active) {
            el.interval = setInterval(() => el.time++, 1000);
          } else {
            clearInterval(el.interval);
          }
        }
      });
    },
    stopTimer(id) {
      this.timers.forEach(el => {
        if (el.id === id) {
          el.active = false
          el.time = 0
          clearInterval(el.interval)
        }
      });
    }
  }
}
</script>

<template>
  <div class="container">
    <timer
      v-for="item in timers"
      :key="item.id"
      :timer="item"
      @start="startTimer"
      @stop="stopTimer"
    />
    <button @click="addTimer" class="app-button" type="button" >+</button>
  </div>
</template>

<style lang="scss" scoped>
.container {
  display: grid;
  grid-template-columns: repeat(3, 225px);
  grid-auto-rows: 120px;
  gap: 45px 50px;

  @include tablets {
    grid-template-columns: repeat(2, 225px);
  }

  @include phones {
    grid-template-columns: 225px;
  }
}

.app-button {
  font-size: 40px;
  color: $color-gray-light;
  background-color: $color-gray;
  transition: 0.3s;
  
  &:hover {
    color: $color-gray-dark;
    box-shadow: 0px 0px 10px 2px $color-gray-light;
  }

  &:active {
    box-shadow: none;
  }
}
</style>
