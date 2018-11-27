<template>
  <div id="app">
    <h1>⏱ 5 minute feature</h1>
    <div v-show="!finished">
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
          :transform="countdownTransform"
          :time="activeTime"
          :auto-start="false"
          @end="onCountdownEnd()"
          >
          <template slot-scope="props">
            <div class="countdown-display">{{props.minutes}}:{{props.seconds}}</div>
            <div>remaining</div>
          </template>
        </Countdown>
      </div>
      <div class="container">
        <button @click="toggleCountdown()">
          {{ countingDown ? "Stop" : "Start" }}
        </button>
        <div class="container">
          <button :disabled="!featureName" @click="done()">
            Done!
          </button>
          <button @click="resetCountdownTimer()">
            Reset
          </button>
        </div>
      </div>
    </div>
    <div v-show="finished" class="container">
      <button @click="undone()">&larr; Back</button>
      <div class="success-container">
        <p>You finished "{{ featureName }}"</p>
        <div class="share-container">
          <TwitterShare
            :text="shareText"
          >
            Let Twitter know
          </TwitterShare>
        </div>
      </div>
      <div class="container">
        <button @click="reset()">
          Ship another feature
        </button>
      </div>
    </div>
    <div class="footer">
      <h3>Enjoy using 5 minute feature?</h3>
      <a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/hugodf"><img src="https://www.buymeacoffee.com/assets/img/BMC-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px">Buy me a coffee</span></a>
      <p>Built by <a href="https://codewithhugo.com">Hugo</a>, <a href="https://twitter.com/hugo__df/status/1067509921082720256">Credits</a></p>
    </div>
  </div>
</template>

<script>
import FeatureNameInput from '@/components/FeatureNameInput.vue'
import TimeSelector from '@/components/TimeSelector.vue'
import Countdown from '@chenfengyuan/vue-countdown'
import TwitterShare from '@/components/TwitterShare.vue'

export default {
  name: 'app',
  components: {
    FeatureNameInput,
    TimeSelector,
    Countdown,
    TwitterShare
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
    countdownTransform(props) {
      Object.entries(props).forEach(([key, value]) => {
        // Adds leading zero
        const digits = value < 10 ? `0${value}` : value;

        // uses singular form when the value is less than 2
        const word = value < 2 ? key.replace(/s$/, '') : key;

        props[key] = digits;
      });

      return props;
    },
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
      this.countingDown = false
      this.resetCountdownTimer()
    },
    resetCountdownTimer() {
      // hack to reset the countdown timer
      this.activeTime++
      this.$nextTick(() => {
        this.activeTime--
      })
    },
    done() {
      this.finished = true
      this.countingDown = false
    },
    undone() {
      this.finished = false
    }
  },
  computed: {
    shareText() {
      return `"${this.featureName}" got shipped in ${this.activeTimeName}`
    }
  },
  watch: {
    countingDown(val) {
      if(val) {
        this.$refs.countdown.start()
      } else {
        this.$refs.countdown.abort()
      }
    }
  }
}
</script>

<style>
#app {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  text-align: center;
  padding-top: 2em;
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
  padding-top: 2em;
  padding-bottom: 2em;
}
button {
  padding: .5em 1em;
  margin: .1em;
  background-color: white;
  border: solid 1px lightgrey;
  font-size: 1em;
}
.countdown-display {
  font-size: 2em;
  margin-bottom: .25em;
}
.bmc-button img{
  width: 27px !important;
  margin-bottom: 1px !important;
  box-shadow: none !important;
  border: none !important;
  vertical-align: middle !important;
}
.bmc-button{
  line-height: 36px !important;
  height:37px !important;
  text-decoration: none !important;
  display:inline-flex !important;
  color:#ffffff !important;
  background-color:#000000 !important;
  border-radius: 3px !important;
  border: 1px solid transparent !important;
  padding: 1px 9px !important;
  font-size: 22px !important;
  letter-spacing: 0.6px !important;
  box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;
  margin: 0 auto !important;
  font-family: cursive !important;
  box-sizing: border-box !important;
  transition: 0.3s all linear !important;
}
.bmc-button:hover, .bmc-button:active, .bmc-button:focus {
  text-decoration: none !important;
  box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;
  opacity: 0.85 !important;
  color:#ffffff !important;
}
.footer {
  margin-top: 7em;
}
</style>
