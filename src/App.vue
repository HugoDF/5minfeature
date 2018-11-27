<template>
  <div id="app">
    <Countdown :time="1000000">
      <template slot-scope="props">
        {{props.minutes}}:{{props.seconds}} left
      </template>
    </Countdown>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Countdown from '@xkeshi/vue-countdown'

export default {
  name: 'app',
  components: {
    Countdown
  }
}
</script>

<style>
#app {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  text-align: center;
}
</style>
