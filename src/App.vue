<template>
  <div id="app">
    <header>
      <div class="header__wrapper">
        <div class="header__icon">
          <img src="./assets/mainLogo.png" alt="Система таблиц логотип">
          <div class="header__text">Система таблиц</div>
        </div>
        <div class="header__user">
          <div>
            <div class="header__username">Светлана</div>
            <div class="header__post">Сотрудник</div>
          </div>
          <img class="header__profile-icon" src="./assets/icons/userIcon.png" alt="userIcon">
        </div>
      </div>

    </header>
    <div class="app__wrapper">
      <h1 class="app__title">Список заявлений</h1>
      <Search
        class="app__search"
        @search="changeSearchText"
      />
      <MobileTable
        v-if="isMobile"
        :data="tableData"
        :fields="header"
        :sort-by="sortBy"
        @changeSort="mobileSort"
        @sort="changeSort"
      />
      <Table
        v-else
        :data="tableData"
        :header="header"
        :sort-by="sortBy"
        @sort="changeSort"
      />
    </div>
  </div>
</template>

<script>
import Search from './components/Search.vue';
import Table from './components/Table.vue';
import MobileTable from './components/MobileTable.vue';

export default {
  name: 'App',
  components: {
    Search,
    Table,
    MobileTable,
  },
  data: () => ({
    searchText: '',
    isMobile: false,
    sortBy: {
      field: 'none',
      type: 'none',
    },
    tableData: [],
    data: [
      {
        id: 1,
        name: 'Соколова София Михайловна',
        date: '2023-01-25',
        subjects: [{
          subject: 'Русский язык',
          score: '4.3',
        },
        {
          subject: 'Математика',
          score: '4',
        },
        {
          subject: 'Информатика',
          score: '5',
        },
        ],
      },
      {
        id: 2,
        name: 'Павлов Владислав Эминович',
        date: '2023-02-24',
        subjects: [{
          subject: 'Русский язык',
          score: '5',
        },
        {
          subject: 'Математика',
          score: '4',
        },
        {
          subject: 'Информатика',
          score: '5',
        },
        ],
      },
      {
        id: 3,
        name: 'Филиппов Семён Глебович',
        date: '2023-01-22',
        subjects: [{
          subject: 'Русский язык',
          score: '4',
        },
        {
          subject: 'Математика',
          score: '3',
        },
        {
          subject: 'Информатика',
          score: '5',
        },
        ],
      },
      {
        id: 4,
        name: 'Щукина Мария Викторовна',
        date: '2023-03-20',
        subjects: [{
          subject: 'Русский язык',
          score: '3.2',
        },
        {
          subject: 'Математика',
          score: '3',
        },
        {
          subject: 'Информатика',
          score: '2.5',
        },
        ],
      },
      {
        id: 5,
        name: 'Потапова Вера Михайловна',
        date: '2023-01-21',
        subjects: [{
          subject: 'Русский язык',
          score: '3',
        },
        {
          subject: 'Математика',
          score: '5',
        },
        {
          subject: 'Информатика',
          score: '4',
        },
        ],
      },
      {
        id: 6,
        name: 'Ефремова Стефания Максимовна',
        date: '2023-02-02',
        subjects: [{
          subject: 'Русский язык',
          score: '4',
        },
        {
          subject: 'Математика',
          score: '4',
        },
        {
          subject: 'Информатика',
          score: '3',
        },
        ],
      },
      {
        id: 7,
        name: 'Сычев Василий Михайлович',
        date: '2023-02-02',
        subjects: [{
          subject: 'Русский язык',
          score: '4.3',
        },
        {
          subject: 'Математика',
          score: '4.8',
        },
        {
          subject: 'Информатика',
          score: '5',
        },
        ],
      },
      {
        id: 8,
        name: 'Соколова Полина Арсентьевна',
        date: '2023-01-29',
        subjects: [{
          subject: 'Русский язык',
          score: '4',
        },
        {
          subject: 'Математика',
          score: '3',
        },
        {
          subject: 'Информатика',
          score: '3',
        },
        ],
      },
      {
        id: 9,
        name: 'Шаповалов Артемий Сергеевич',
        date: '2023-01-20',
        subjects: [{
          subject: 'Русский язык',
          score: '5',
        },
        {
          subject: 'Математика',
          score: '5',
        },
        {
          subject: 'Информатика',
          score: '5',
        },
        ],
      },
      {
        id: 10,
        name: 'Александрова Милана Тимуровна',
        date: '2023-01-31',
        subjects: [
          {
            subject: 'Русский язык',
            score: '4.5',
          },
          {
            subject: 'Математика',
            score: '3',
          },
          {
            subject: 'Информатика',
            score: '3',
          },
        ],
      },
    ],
    header: [
      {
        caption: 'ФИО',
        value: 'name',
      },
      {
        caption: 'Дата подачи',
        value: 'date',
      },
      {
        caption: 'Балл по русскому',
        value: 'subjects-ru',
      },
      {
        caption: 'Балл по математике',
        value: 'subjects-math',
      },
      {
        caption: 'Балл по информатике',
        value: 'subjects-cs',
      },
      {
        caption: 'Суммарный балл',
        value: 'sum',
      },
      {
        caption: 'Процент',
        value: 'procent',
        width: '75px',
      },
    ],
  }),
  watch: {
    searchText(val) {
      if (val.length > 0) {
        const reg = new RegExp(`${val}`, 'gi');
        const result = this.data.filter((item) => reg.test(item.name));
        this.changeTableData(result);
      } else {
        this.sort(this.sortBy.type, this.sortBy.field);
      }
    },
  },
  mounted() {
    this.checkMobile();
    this.$nextTick(() => {
      window.addEventListener('resize', this.checkMobile);
    });
    const sort = JSON.parse(localStorage.getItem('sortBy')) || { field: 'name', type: 'none' };
    this.setProcentValue();
    this.setSumValue();
    this.changeSortBy(sort);
    this.sort(sort.type, sort.field);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.checkMobile);
  },
  methods: {
    checkMobile() {
      this.isMobile = window.innerWidth <= 425;
    },
    setProcentValue() {
      this.data.forEach((item) => {
        let procent = 0;
        item.subjects.forEach((obj) => procent += obj.score * 6.66);
        item.procent = Math.round(procent);
      });
    },
    setSumValue() {
      this.data.forEach((item) => {
        let sum = 0;
        item.subjects.forEach((item) => sum += parseFloat(item.score));
        item.sum = sum;
      });
    },
    changeSearchText(val) {
      this.searchText = val;
    },
    changeSortBy(val) {
      this.sortBy = val;
      localStorage.setItem('sortBy', JSON.stringify(this.sortBy));
    },
    sortValues(field, type, a, b) {
      let valueA; let
        valueB;

      switch (field) {
        case 'subjects-cs':
          valueA = a.subjects[2].score;
          valueB = b.subjects[2].score;
          break;
        case 'subjects-ru':
          valueA = a.subjects[0].score;
          valueB = b.subjects[0].score;
          break;
        case 'subjects-math':
          valueA = a.subjects[1].score;
          valueB = b.subjects[1].score;
          break;
        default:
          valueA = typeof (a[field]) === 'string' ? a[field].toLocaleLowerCase() : a[field];
          valueB = typeof (b[field]) === 'string' ? b[field].toLocaleLowerCase() : b[field];
          break;
      }

      switch (type) {
        case 'ASC':
          if (valueA < valueB) return -1;
          if (valueA > valueB) return 1;
          return 0;
        case 'DESC':
          if (valueA > valueB) return -1;
          if (valueA < valueB) return 1;
          return 0;
      }
    },
    changeSort(field) {
      if (field !== this.sortBy.field) {
        this.changeSortBy({ field, type: 'ASC' });
      } else {
        switch (this.sortBy.type) {
          case 'ASC':
            this.changeSortBy({ field, type: 'DESC' });
            break;
          case 'DESC':
            this.changeSortBy({ field, type: 'none' });
            break;
          default:
            this.changeSortBy({ field, type: 'ASC' });
            break;
        }
      }
      this.sort(this.sortBy.type, field);
    },
    sort(type, field) {
      if (type !== 'none') {
        this.changeTableData([...this.data].sort((a, b) => this.sortValues(field, type, a, b)));
      } else {
        this.changeTableData([...this.data]);
      }
    },
    changeTableData(data) {
      this.tableData = data;
    },
    mobileSort(type) {
      const sortObj = { ...this.sortBy };
      sortObj.type = this.sortBy.type === type ? 'none' : type;
      this.changeSortBy(sortObj);
      this.sort(sortObj.type, sortObj.field);
    },
  },
};
</script>

<style lang="sass">
@import './assets/styles/colors.sass'

@font-face
  font-family: 'Proxima Nova'
  src: url('./assets/fonts/proximaNova/proximanova_regular.ttf') format('truetype')
  font-style: normal
  font-weight: 500
  font-stretch: normal

body
  margin: 0
  background-color: $blue-super-light

header
  width: 100%
  height: 84px
  background-color: $white
  display: flex
  align-items: center
.header
  &__wrapper
    width: 100%
    display: flex
    justify-content: space-between
    align-items: center
    margin-left: 60px
    margin-right: 60px
  &__icon
    width: 130px
    color: $blue
    font-weight: 800
    text-transform: uppercase
    display: flex
    text-align: start
  &__text
    line-height: 16px
    font-size: 16px
    margin-left: 10px
  &__user
    display: flex
    text-align: end
  &__username
    font-weight: 700
    color: $blue
  &__post
    font-weight: 400
    color: $medium
  &__profile-icon
    width: 44px
    height: 44px
    border-radius: 50%
    margin-left: 8px
#app
  font-family: 'Proxima Nova', Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  color: #2c3e50
.app
  &__wrapper
    margin-top: 43px
    margin-left: 60px
    margin-right: 60px
  &__title
    margin: 0
    margin-bottom: 20px
  &__search
    margin-bottom: 20px
@media (max-width: 1366px)
  .header
    &__wrapper
      margin-left: 40px
      margin-right: 40px
  .app__wrapper
    margin-left: 40px
    margin-right: 40px
@media (max-width: 768px)
  .header
    &__wrapper
      margin-left: 12px
      margin-right: 12px
  .app__wrapper
    margin-left: 12px
    margin-right: 12px
</style>
