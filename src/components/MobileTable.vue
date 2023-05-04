<template>
  <section>
    <div
      class="mobile-table__select"
    >
      <Select
        :variants="fields"
        :value="sortBy.field"
        @change="sort"
      />
      <div>
        <SortButton
          class="mobile-table__sortBtn"
          :isActive="checkSortDESC"
          @click="changeSort('DESC')"
        />
        <SortButton
          class="mobile-table__sortBtn_ASC"
          :isActive="checkSortASC"
          @click="changeSort('ASC')"
        />
      </div>
    </div>
    <div class="mobile-table__wrapper">
      <div
        v-for="(item, index) in data"
        :key="index"
        class="mobile-table__card"
      >
        <div
          v-for="(field, fieldIndex) in fields"
          :key="fieldIndex"
          class="mobile-table__field"
        >
          <div class="mobile-table__field-block mobile-table__field-block_caption">
            {{ field.caption }}
          </div>
          <div class="mobile-table__field-block">
            <Progress
              v-if="isProcent(field.value)"
              :procent="getValue(field.value, item)"
              :isMobile="true"
            />
            <p
              v-else
              :class="getColor(field.value, getValue(field.value, item))"
            >
              {{ getValue(field.value, item) }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Select from './Select.vue';
import SortButton from './SortButton.vue';
import Progress from './Progress.vue';

export default {
  name: 'MobileTable',
  props: {
    data: {
      type: Array,
      required: true,
    },
    fields: {
      type: Array,
      required: true,
    },
    sortBy: {
      type: Object,
      required: true,
    },
  },
  components: {
    Select,
    SortButton,
    Progress,
  },
  computed: {
    checkSortDESC() {
      return this.sortBy.type === 'DESC';
    },
    checkSortASC() {
      return this.sortBy.type === 'ASC';
    },
  },
  methods: {
    isProcent(value) {
      return value === 'procent';
    },
    sort(value) {
      this.$emit('sort', value);
    },
    changeSort(type) {
      this.$emit('changeSort', type);
    },
    getValue(value, item) {
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
    getColor(type, value) {
      const correct = ['subjects-ru', 'subjects-math', 'subjects-cs'].some(
        (item) => item === type,
      );

      if (correct) {
        let style = 'mobile-table__text';
        style += value >= 3
          ? value >= 4 ? ` ${style}_green` : ` ${style}_orange`
          : ` ${style}_red`;
        return style;
      }
    },
  },
};
</script>

<style lang="sass" scoped>
@import '../assets/styles/colors.sass'

.mobile-table
  &__select
    display: flex
    justify-content: space-between
    margin-bottom: 8px
  &__sortBtn
    margin-right: 4px
    &_ASC
      transform: rotate(180deg)
  &__card
    margin-bottom: 8px
  &__field
    padding: 0 12px 0 12px
    display: flex
    justify-content: space-between
    align-items: center
    min-height: 33px
    background-color: $white
    margin-bottom: 2px
    &:last-child
      margin-bottom: 0
    &-block
      width: 45%
      &_caption
        color: $medium
  &__text
    &_red
      color: $red
    &_orange
      color: $orange
    &_green
      color: $green
</style>
