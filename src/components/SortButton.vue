<template>
  <button
    :class="getClass"
    @click="clickEvent()"
  >
    <svg width="7" height="8" viewBox="0 0 7 8" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M3.5 8L7 4L4.76995e-08 4L3.5 8Z" :fill="color"/>
      <path d="M3.5 0L3.5 4" :stroke="color" stroke-width="2"/>
    </svg>
  </button>
</template>

<script>
export default {
  name: 'SortButtonComponent',
  data: () => ({
    color: '#006CFE',
  }),
  props: {
    isActive: {
      type: Boolean,
      default: false,
    },
  },
  mounted() {
    this.changeColor();
  },
  computed: {
    getClass() {
      const baseClass = 'sort-btn';
      return `${baseClass} ${this.isActive ? 'sort-btn_active' : ''}`;
    },
  },
  watch: {
    isActive() {
      this.changeColor();
    },
  },
  methods: {
    changeColor() {
      this.color = this.isActive ? '#FFFFFF' : '#006CFE';
    },
    clickEvent() {
      this.$emit('click');
    },
  },
};
</script>

<style lang="sass" scoped>
@import '../assets/styles/colors.sass'
.sort-btn
  background-color: transparent
  width: 44px
  height: 44px
  border: 1px solid $blue
  border-radius: 4px
  &_active
    background-color: $blue
</style>
