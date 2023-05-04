<template>
  <td class="cell">
    <p
      v-if="isString"
      :class="color"
    >
      {{ data }}
    </p>
    <Progress
      v-else-if="isProcent"
      :procent="data"
      :is-mobile="isMobile"
    />
  </td>
</template>

<script>
import Progress from './Progress.vue';

export default {
  name: 'Cell',
  components: {
    Progress,
  },
  props: {
    data: {
      type: String | Number,
      required: true,
    },
    type: {
      type: String,
      required: true,
    },
  },
  computed: {
    isString() {
      return this.type !== 'procent';
    },
    isProcent() {
      return this.type === 'procent';
    },
    isMobile() {
      return window.innerWidth <= 425;
    },
    color() {
      const correct = ['subjects-ru', 'subjects-math', 'subjects-cs'].some(
        (item) => item === this.type,
      );
      if (correct) {
        let style = 'cell__text';
        style += this.data >= 3
          ? this.data >= 4 ? ` ${style}_green` : ` ${style}_orange`
          : ` ${style}_red`;
        return style;
      }
    },
  },
};
</script>

<style lang="sass" scoped>
@import '../assets/styles/colors.sass'
.cell
  padding-left: 20px
  height: 62px
  background-color: $white
  &__text
    font-weight: bold
    font-size: 14px
    &_red
      color: $red
    &_orange
      color: $orange
    &_green
      color: $green
</style>
