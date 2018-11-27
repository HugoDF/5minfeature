<template>
  <div id="app">
    <TimeSelector
      :activeTimeName="activeTimeName"
      @selected="onTimeSelected($event)"
      :disabled="countingDown"
      />
    <Countdown
      :time="activeTime"
      leading-zero
      emit-events
      :auto-start="false"
      ref="countdown">>
      <template slot-scope="props">
        {{props.minutes}}:{{props.seconds}} left
      </template>
    </Countdown>
    <div>
      <button @click="toggleCountdown()">
        {{ countingDown ? "Stop" : "Start" }}
      </button>
    </div>
  </div>
</template>

<script>
import Countdown from '@xkeshi/vue-countdown'
import TimeSelector from '@/components/TimeSelector.vue'

export default {
  name: 'app',
  components: {
    Countdown,
    TimeSelector
  },
  data() {
    return {
      activeTimeName: undefined,
      activeTime: undefined,
      countingDown: false
    }
  },
  methods: {
    onTimeSelected({ time, name }) {
      this.activeTime = time
      this.activeTimeName = name
    },
    toggleCountdown() {
      this.countingDown = !this.countingDown
    }
  },
  watch: {
    countingDown(val) {
      if(val) {
        this.$refs.countdown.start()
      } else {
        this.$refs.countdown.pause()
      }
    }
  }
}
</script>

<style>
#app {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  text-align: center;
}
</style>
