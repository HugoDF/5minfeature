<template>
  <div id="app">
    <div class="container">
      <FeatureNameInput
        :feature-name="featureName"
        @change="onFeatureNameChanged($event)"
        :disabled="countingDown"
      />
    </div>
    <div class="container">
      <TimeSelector
        :activeTimeName="activeTimeName"
        @selected="onTimeSelected($event)"
        :disabled="countingDown"
        />
    </div>
    <div class="container">
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
    </div>
    <div class="container">
      <button @click="toggleCountdown()">
        {{ countingDown ? "Stop" : "Start" }}
      </button>
    </div>
  </div>
</template>

<script>
import FeatureNameInput from '@/components/FeatureNameInput.vue'
import TimeSelector from '@/components/TimeSelector.vue'
import Countdown from '@xkeshi/vue-countdown'


export default {
  name: 'app',
  components: {
    FeatureNameInput,
    TimeSelector,
    Countdown,
  },
  data() {
    return {
      activeTimeName: undefined,
      activeTime: undefined,
      countingDown: false,
      featureName: ''
    }
  },
  methods: {
    onTimeSelected({ time, name }) {
      this.activeTime = time
      this.activeTimeName = name
    },
    toggleCountdown() {
      this.countingDown = !this.countingDown
    },
    onFeatureNameChanged(newName) {
      this.featureName = newName
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
.container {
  margin-top: 2em;
  margin-bottom: 2em;
}
</style>
