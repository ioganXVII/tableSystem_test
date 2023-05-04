<template>
  <div
    v-if="isMobile"
    class="progress"
  >
    <div
      class="progress__text"
    >
      {{ `${procent}%` }}
    </div>
    <progress
      max="100"
      :value="procent"
    ></progress>
  </div>
  <div
    v-else
  >
    <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="none" :data-value="procent">
      <circle r="45" cx="50" cy="50" />
      <path ref="progress" class="meter" d="M5,50a45,45 0 1,0 90,0a45,45 0 1,0 -90,0" stroke-linecap="round"
        stroke-linejoin="round" stroke-dashoffset="282.78302001953125" stroke-dasharray="282.78302001953125" />
      <text x="50" y="50" text-anchor="middle" dominant-baseline="central" font-size="20"></text>
    </svg>
  </div>
</template>

<script>

export default {
  name: 'Progress',
  props: {
    procent: {
      type: String | Number,
      default: 0,
    },
    isMobile: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    calculate() {
      if (!this.isMobile) {
        const path = this.$refs.progress;
        const length = path.getTotalLength();

        const to = length * ((100 - this.procent) / 100);
        path.getBoundingClientRect();

        path.style.strokeDashoffset = Math.max(0, to); path.nextElementSibling.textContent = `${this.procent}%`;
      }
    },
  },
  watch: {
    procent() {
      this.calculate();
    },
  },
  mounted() {
    this.calculate();
  },
};
</script>

<style lang="sass" scoped>
@import '../assets/styles/colors.sass'

.progress
  max-width: 200px
  display: flex
  align-items: center
  &__text
    width: 40px
progress
  background-color: $blue-light
  border: none
  margin-left: 4px
  border-radius: 15px
  max-width: 70%
  height: 4px
  &[value^="3"]:not([value="3"]),
  &[value^="4"]:not([value="4"])
    &::-webkit-progress-value
      background-color: $orange
    &::-moz-progress-bar
      background-color: $orange
  &[value^="5"]:not([value="5"]),
  &[value^="6"]:not([value="6"]),
  &[value^="7"]:not([value="7"]),
  &[value^="8"]:not([value="8"]),
  &[value^="9"]:not([value="9"]),
  &[value="100"]
    &::-webkit-progress-value
      background-color: $green
    &::-moz-progress-bar
      background-color: $green

  &::-webkit-progress-bar
    background-color: $blue-light
    border-radius: 7px

  &::-webkit-progress-value
    border-radius: 15px
    background-color: $red

  &::-moz-progress-bar
    background-color: $red

svg
  display: inline-flex
  vertical-align: bottom
  width: 50px
  height: 50px
  &[data-value^="3"]:not([data-value="3"]),
  &[data-value^="4"]:not([data-value="4"])
    .meter
      stroke: $orange
  &[data-value^="5"]:not([data-value="5"]),
  &[data-value^="6"]:not([data-value="6"]),
  &[data-value^="7"]:not([data-value="7"]),
  &[data-value^="8"]:not([data-value="8"]),
  &[data-value^="9"]:not([data-value="9"]),
  &[data-value="100"]
    .meter
      stroke: $green

circle
  stroke: $blue-light
  stroke-width: 1px
  stroke-dasharray: 0
  fill: none

.meter
  stroke-width: 5px
  stroke: $red
  fill: none
  transition: stroke-dashoffset 1s cubic-bezier(0.43, 0.41, 0.22, 0.91)
  transform-origin: center center
  transform: rotate(-90deg) scaleX(-1)
</style>
