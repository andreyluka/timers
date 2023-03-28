<script>
export default {
  name: 'timer',
  props: {
    timer: Object
  },
  computed: {
    formatTime() {
      const totalMinutes = Math.floor(this.timer.time / 60);
      const seconds = this.timer.time - totalMinutes * 60;
      const hours = Math.floor(totalMinutes / 60);
      const minutes = totalMinutes - hours * 60;

      const formatSeconds = seconds < 10 ? `0${seconds}` : seconds;
      const formatMinutes = minutes < 10 ? `0${minutes}` : minutes;

      let correctTime;

      if (hours > 0) {
        correctTime = `${hours}:${formatMinutes}:${formatSeconds}`;
      } else if (minutes > 0) {
        correctTime = `${formatMinutes}:${formatSeconds}`;
      } else {
        correctTime = `${seconds}`;
      }
      
      return correctTime;
    }
  }
}
</script>

<template>
  <div class="timer" :class="{'timer--active' : timer.active}">
    <div class="timer__time">{{ formatTime }}</div>
    <div class="timer__btns">
      <button 
        class="timer__btn timer__btn-start"
        @click.prevent="$emit('start', timer.id)" 
        type="button"
        ></button>
      <button 
        class="timer__btn timer__btn-stop"
        @click.prevent="$emit('stop', timer.id)" 
        type="button" 
      ></button>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.timer {
  display: flex;
  flex-direction: column;
  background-color: $color-gray;
}

.timer__time, .timer__btns {
  display: flex;
  justify-content: center;
  align-items: center;
  flex: 1;
  font-size: 22px;
}

.timer__time {
  border-bottom: 1px solid $color-gray-light;
}

.timer__btns {
  display: flex;
  justify-content: center;
  border-top: 1px solid $color-gray-light;
}

.timer__btn {
  margin-right: 48px;
  transition: 0.3s;

  &:last-child {
    margin-right: 0;
  }
  
  &-start {
    width: 17px;
    height: 20px;
    position: relative;

    &::before {
      content: '';
      border: 10px solid transparent;
      border-left: 17px solid $color-gray-light;
      border-right: none;
      position: absolute;
      top: 0;
      left: 0;
      transition: 0.3s;
    }

    &:hover {
      &::before {
        border-left-color: $color-gray-dark;
      }
    }
  }
  
  &-stop {
    width: 20px;
    height: 20px;
    background-color: $color-gray-light;
  
    &:hover {
      background-color: $color-gray-dark;
    }
  }
}

.timer--active {
  color: $color-white;
  
  .timer__btn-start {
    
    &::before, &::after {
      content: '';
      height: 20px;
      width: 3px;
      background-color: $color-white;
      position: absolute;
      top: 0;
      transition: 0.3s;
    }
    
    &::before {
      border: none;
      left: 4px;
    }
    
    &::after {
      left: 11px;
    }

    &:hover {
      &::before, &::after {
        background-color: $color-gray-dark;
      }
    }
  }
  
  .timer__btn-stop {
    background-color: $color-white;
  
    &:hover {
      background-color: $color-gray-dark;
    }
  }
}
</style>
