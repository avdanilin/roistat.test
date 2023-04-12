<template>
  <div class="table-employees">
    <div class="table-employees__head">
      <div class="table-employees__head-row">
        <h6 :class="['table-employees__head-column', { 'table-employees__head-column_active' : directionName === 'name' && stateDirectionSort }]" @click="sortBy('name')">Имя</h6>
        <h6 :class="['table-employees__head-column', { 'table-employees__head-column_active' : directionName === 'phone' && stateDirectionSort }]" @click="sortBy('phone')">Телефон</h6>
      </div>
    </div>
    <div class="table-employees__body">
      <div class="table-employees__row">
        <treeEmployers v-for="user in users" :user-id="user.id" :key="user.id" :user="user"/>
      </div>
    </div>
  </div>
</template>

<script>
import TreeEmployers from './treeEmployers.vue'

export default {
  name: 'tableEmployees',
  components: {TreeEmployers},
  props: {
    users: {
      type: Array,
      default: () => []
    }
  },
  data: () => ({
    hasBossState: null,
    sortKey: '',
    directionName: '',
    sortDirection: 1,
    stateDirectionSort: false
  }),
  methods: {
    sortBy (key) {
      this.directionName = key
      this.stateDirectionSort = !this.stateDirectionSort

      if (this.sortKey === key) {
        this.sortDirection = this.sortDirection * -1
      } else {
        this.sortKey = key
        this.sortDirection = 1
      }

      this.users.sort((a, b) => {
        const modifier = this.sortDirection === 1 ? 1 : -1

        if (a[this.sortKey] < b[this.sortKey]) {
          return -1 * modifier
        }

        if (a[this.sortKey] > b[this.sortKey]) {
          return 1 * modifier
        }

        return 0
      })
    }
  }
}
</script>

<style scoped>
.table-employees {
  display: flex;
  flex-wrap: wrap;
  width: calc(100% - 1rem);
  margin: 0;
}

.table-employees__head {
  width: 100%;
}

.table-employees__head-column {
  display: flex;
  align-items: center;
  gap: 1.25rem;
  background-color: #f2f2f2;
  text-align: left;
  padding: 0.25rem;
  cursor: pointer;
  flex-basis: 50%;
  margin: 0;
}

.table-employees__head-column::after {
  content: '☝';
  font-size: 1rem;
  transition: transform .3s ease-in-out;
  transform: scale(1);
}

.table-employees__head-column_active::after {
  transform: scale(-1);
}

.table-employees__row,
.table-employees__head-row {
  display: flex;
}

.table-employees__row {
  flex-wrap: wrap;
  width: calc(100% - 2px);
  border: 1px solid #ddd;
}

.table-employees__body {
  flex-basis: 100%;
}

.table-employees__head-column:not(:last-child) {
  border-right: 1px solid #ddd;
}
</style>
