<template>
  <div class="select__wrapper">
    <select
      ref="select"
      class="select__input"
      :value="value"
      aria-label="Выбор поля сортировки"
      @change="changeValue($event.target.value)"
    >
      <option
        v-for="(item, index) in variants"
        :key="index"
        class="select__option"
        :value="item.value"
      >
        {{ item.caption }}
      </option>
    </select>
    <div class="select__icon">
      <svg width="9" height="5" viewBox="0 0 9 5" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M9 0H0L4.5 5L9 0Z" fill="#006CFE"/>
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SelectComponent',
  props: {
    value: {
      type: String,
      required: true,
    },
    variants: {
      type: Array,
      required: true,
    },
  },
  methods: {
    changeValue(value) {
      this.$emit('change', value);
    },
  },
};
</script>

<style lang="sass" scoped>
@import '../assets/styles/colors.sass'

.select
  &__wrapper
    display: flex
    justify-content: center
    position: relative

  &__input
    height: 46px
    border: 2px solid
    border-color: $blue-light
    border-radius: 4px
    appearance: none
    background-color: $white
    font-family: 'Proxima Nova', sans-serif
    font-size: 14px
    line-height: 17px
    padding: 13px 0 13px 12px
    padding-right: 120px
    border-radius: 4px
    &:focus-visible
      outline-color: $blue

  &__icon
    pointer-events: none
    width: 50px
    height: 100%
    position: absolute
    right: 0
    display: flex
    align-items: center
    justify-content: center
@media (max-width: 425px)
  .select
    &__input
      padding-right: 90px
@media (max-width: 375px)
  .select
    &__input
      padding-right: 35px
@media (max-width: 320px)
  .select
    &__input
      padding-right: 15px
</style>
