<template>
  <div id="app">
    <div v-if="!finished">
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
          ref="countdown"
          :leading-zero="true"
          :emit-events="true"
          :time="activeTime"
          :auto-start="false"
          @countdownend="onCountdownEnd()"
          >
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
    <div v-else class="container success-container">
      <p>You finished "{{ featureName }}"</p>
      <div class="share-container">
        <TwitterShare
          :text="shareText"
        >
          Let Twitter know
        </TwitterShare>
        
        <button @click="reset()">
          Ship another feature
        </button>
      </div>
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
    TwitterShare: () => import('@/components/TwitterShare.vue')
  },
  data() {
    return {
      activeTimeName: undefined,
      activeTime: undefined,
      countingDown: false,
      featureName: '',
      finished: false,
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
    },
    onCountdownEnd() {
      this.finished = true
      this.countingDown = false
    },
    reset() {
      this.finished = false
      this.featureName = ''
    }
  },
  computed: {
    shareText() {
      return `${this.featureName} got shipped in ${this.activeTimeName}`
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
.share-container {
  display: flex;
  justify-content: center;
}
.success-container {
  margin-top: 10em;
}
</style>
