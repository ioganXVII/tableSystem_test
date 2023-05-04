<template>
  <div>
    <div
      class="search"
      @click="setFocus"
     >
      <svg width="15" height="16" viewBox="0 0 15 16" fill="none" xmlns="http://www.w3.org/2000/svg">
        <circle cx="7" cy="7" r="6.5" stroke="#006CFE"/>
        <line x1="11.3536" y1="11.6464" x2="14.6464" y2="14.9393" stroke="#006CFE"/>
      </svg>
      <input
        class="search__input"
        ref="input"
        type="text"
        placeholder="Поиск"
        @input="search($event.target.value)"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Search',
  methods: {
    setFocus() {
      this.$refs.input.focus();
    },
    search(val) {
      this.$emit('search', val);
    },
  },
};
</script>

<style lang="sass" scoped>
@import '../assets/styles/colors.sass'

.search
  display: flex
  padding: 16px
  border-radius: 4px
  border-style: solid
  border-color: $blue-light
  background-color: $white
  &__input
    font-family: 'Proxima Nova'
    margin-left: 5px
    border: none
    outline: none
    &:focus-visible
      outline-color: field
  &:focus-within
    border-color: $blue !important
  &:hover
    border-color: $blue-middle
    cursor: text
</style>
