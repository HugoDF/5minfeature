<template>
  <div>
    <div v-for="option in timeOptions" :key="option.value">
      <input  
        v-model="activeTime"
        type="radio"
        name="time-selector"
        :id="option.labelText"
        :value="option.value"
        :disabled="disabled"
        >
      <label :for="option.labelText">{{option.labelText}}</label>
    </div>
  </div>
</template>

<script>
const FIVE_MINUTES_IN_MILLISECONDS = 5 * 60 * 1000
const TEN_MINUTES_IN_MILLISECONDS = FIVE_MINUTES_IN_MILLISECONDS * 2
const THIRTY_MINUTES_IN_MILLISECONDS = TEN_MINUTES_IN_MILLISECONDS * 3
const TIME_OPTIONS = [
  { value: FIVE_MINUTES_IN_MILLISECONDS, labelText: "5min" },
  { value: TEN_MINUTES_IN_MILLISECONDS, labelText: "10min" },
  { value: THIRTY_MINUTES_IN_MILLISECONDS, labelText: "30min" }
]

export default {
  props: {
    activeTimeName: {
      type: String,
      default: '5min'
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      timeOptions: TIME_OPTIONS,
      time: FIVE_MINUTES_IN_MILLISECONDS
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
          name: TIME_OPTIONS.find(({ value }) => value === time).labelText
        })
      }
    }
  }
};
</script>

<style>
</style>