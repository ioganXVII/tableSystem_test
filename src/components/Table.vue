<template>
  <section class="table">
    <table
      class="table__wrapper"
    >
      <thead>
        <tr>
          <th
            v-for="(item, index) in header"
            :key="index"
            class="table__head-elem"
            @click="sort(item.value)"
          >
            <div
              class="table__head-content"
              :style="getHeaderCellStyle(item.width)"
            >
              {{ item.caption }}
              <svg
                v-if="showSort(item.value)"
                :style="getDirection"
                width="7" height="8" viewBox="0 0 7 8" fill="none" xmlns="http://www.w3.org/2000/svg"
              >
                <path d="M3.5 8L7 4L4.76995e-08 4L3.5 8Z" fill="#006CFE"/>
                <path d="M3.5 0L3.5 4" stroke="#006CFE" stroke-width="2"/>
              </svg>
            </div>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(item, index) in data"
          :key="index"
        >
          <Cell
            v-for="(headItem, headIndex) in header"
            :key="headIndex"
            class="table__cell"
            :data="getItemValue(item, headItem.value)"
            :type="headItem.value"
          />
        </tr>
      </tbody>
    </table>
  </section>
</template>

<script>
import Cell from './Cell.vue';

export default {
  name: 'Table',
  props: {
    header: {
      type: Array,
      required: true,
    },
    data: {
      type: Array,
      required: true,
    },
    sortBy: {
      type: Object,
      required: true,
    },
  },
  components: {
    Cell,
  },
  computed: {
    getDirection() {
      return this.sortBy.type === 'ASC' ? 'transform: rotate(180deg)' : '';
    },
  },
  methods: {
    getHeaderCellStyle(value) {
      return value ? `width: ${value}` : '';
    },
    sort(value) {
      this.$emit('sort', value);
    },
    showSort(headerVal) {
      return this.sortBy.type !== 'none' && this.sortBy.field === headerVal;
    },
    getItemValue(item, value) {
      switch (value) {
        case 'date':
          return new Date(item[value]).toLocaleDateString('ru-RU');
        case 'subjects-ru':
          return item.subjects[0].score;
        case 'subjects-math':
          return item.subjects[1].score;
        case 'subjects-cs':
          return item.subjects[2].score;
        default:
          return item[value];
      }
    },
  },
};
</script>

<style lang="sass" scoped>
@import '../assets/styles/colors.sass'
.table
  width: 100%
  &__wrapper
    width: 100%
  &__head
    &-elem
      text-align: start
      padding-left: 20px
      color: $blue
      &:hover
        cursor: pointer
        color: $blue-dark
    &-content
      width: 200px
@media (max-width: 1440px)
  .table
    overflow: scroll
</style>
