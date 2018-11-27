<template>
  <div class="feature-name-container">
    <input
      :disabled="disabled"
      class="feature-name-input"
      placeholder="🆕 feature name"
      type="text"
      v-model="internalFeatureName"
      />
  </div>
</template>

<script>
export default {
  props: {
    featureName: {
      type: String,
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    internalFeatureName: {
      get() {
        return this.featureName
      },
      set(newName) {
        this.$emit('change', newName)
      }
    }
  }
}
</script>

<style>
.feature-name-container {
  text-align: center;
}
.feature-name-input {
  text-align: center;
  font-size: 1.5em;
  width: 70%;
  border: none;
  padding: .5em;
}
@media screen and (max-width: 640px) {
  .feature-name-input {
    width: 100%;
  }
}
</style>
