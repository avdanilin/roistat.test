<template>
  <table>
    <thead>
    <tr>
      <th @click="sort('name')">Имя</th>
      <th @click="sort('phone')">Телефон</th>
    </tr>
    </thead>
    <tbody>
  <!--sortedUsers-->
    <tr v-for="user in users" :key="user.id">
      <td>{{ user.name }}</td>
      <td>{{ user.phone }}</td>
<!--      <td>{{ getParentName(user) }}</td>-->
    </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: 'tableEmployees',
  props: {
    users: {
      type: Array,
      default: () => []
    }
  },
  watch: {
    sortedUsers () {
      // const compare = (a, b) => {
      //   if (a.name < b.name) {
      //     return -1
      //   }
      //
      //   if (a.name > b.name) {
      //     return 1
      //   }
      //
      //   return 0
      // }
      //
      // return this.users.sort(compare)
    }
  },
  methods: {
    getParentName (user) {
      const parent = this.users.find(u => u.id === user.parent)
      return parent ? parent.name : '-'
    },
    sort (column) {
      const compare = (a, b) => {
        if (a[column] < b[column]) {
          return -1
        }

        if (a[column] > b[column]) {
          return 1
        }

        return 0
      }

      this.users.sort(compare)
    }
  }
}
</script>

<style scoped>
button {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  margin-bottom: 16px;
}

table {
  border-collapse: collapse;
  width: 100%;
}

th {
  background-color: #f2f2f2;
  text-align: left;
  padding: 8px;
  cursor: pointer;
}

th:hover {
  background-color: #ddd;
}

td {
  border: 1px solid #ddd;
  padding: 8px;
}

tbody tr:hover {
  background-color: #f2f2f2;
}
</style>
