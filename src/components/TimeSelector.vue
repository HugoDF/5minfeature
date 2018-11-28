<template>
  <div>
    <span v-for="option in timeOptions" :key="option.id">
      <div class="radio-container">
        <label v-if="option.custom">Other</label>
        <label v-else :for="option.labelText">{{option.labelText}}</label>
        <input
          v-model="activeTime"
          type="radio"
          name="time-selector"
          :id="option.id"
          :value="option.value"
          :disabled="disabled"
          >
      </div>
      <input
        v-if="option.custom && activeTime === customTime * 60 * 1000"
        id="custom-time"
        class="custom-time-input"
        type="number"
        maxlength="2"
        :max="59"
        :min="0"
        v-model.number="customTime"/>
    </span>
  </div>
</template>

<script>
const FIVE_MINUTES_IN_MILLISECONDS = 5 * 60 * 1000
const TEN_MINUTES_IN_MILLISECONDS = FIVE_MINUTES_IN_MILLISECONDS * 2
const THIRTY_MINUTES_IN_MILLISECONDS = TEN_MINUTES_IN_MILLISECONDS * 3
const TIME_OPTIONS = [
  { id: '5min', value: FIVE_MINUTES_IN_MILLISECONDS, labelText: "5 min" },
  { id: '10min', value: TEN_MINUTES_IN_MILLISECONDS, labelText: "10 min" },
  { id: 'custom', value: THIRTY_MINUTES_IN_MILLISECONDS, labelText: "30 min", custom: true }
]

export default {
  props: {
    activeTimeName: {
      type: String,
      default: '5 min'
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      timeOptions: TIME_OPTIONS,
      time: FIVE_MINUTES_IN_MILLISECONDS,
      customTime: 30
    }
  },
  mounted() {
    this.$emit('selected', {
      time: this.time,
      name: this.activeTimeName
    })
  },
  computed: {
    activeTime: {
      get() {
        return this.time
      },
      set(time) {
        this.time = time
        return this.$emit('selected', {
          time,
          name: this.timeOptions.find(({ value }) => value === time).labelText
        })
      }
    }
  },
  watch: {
    customTime(value) {
      if (value > 59) {
        this.customTime = 59;
      }
      if (value < 0) {
        this.customTime = 0
      }
      const valueInMilliseconds = value * 60 * 1000
      const name = `${value} min`
      this.timeOptions = [
        ...this.timeOptions.map(
          (option) => (option.custom
            ? {
              ...option,
              labelText: name,
              value: valueInMilliseconds
            }
            : option
          )
        )
      ]
      this.time = valueInMilliseconds
      this.$emit('selected', {
        time: valueInMilliseconds,
        name
      })
    }
  }
};
</script>

<style scoped>
.radio-container {
  display: inline-block;
}
.radio-container:not(:last-child) {
  margin-right: 10px;
}
.radio-container {
  padding: 10px;
}
.radio-container > label {
  padding: 5px;
  display: flex;
}
.custom-time-input {
  margin-top: 1em;
  display: flex;
  margin-left: auto;
  margin-right: auto;
  min-width: 75px;
  text-align: center;
  font-size: 1em;
}
</style>
