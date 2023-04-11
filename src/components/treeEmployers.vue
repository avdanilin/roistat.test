<template>
  <div class="tree-employers">
    <div class="tree-employers__body">
      <div class="tree-employers__column">
        {{ user.name }}
      </div>
      <div class="tree-employers__column">
        {{ user.phone }}
      </div>
    </div>
    <!--TODO: стили для колонок или переделать на таблицу? и аккордионны-->
    <div v-if="user.children.length > 0" class="tree-employers__row">
      <tree-employers v-for="user in user.children" :user-id="user.id" :key="user.id" :user="user"/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'treeEmployers',
  props: {
    user: {
      type: Object,
      required: true
    },
    userId: {
      type: Number
    }
  }
}
</script>

<style scoped>
.tree-employers {
  flex-basis: 100%;
}

.tree-employers:not(:last-child) {
  border-bottom: 1px solid #ddd;
}

.tree-employers__body {
  display: flex;
}

.tree-employers__column {
  flex-basis: calc(50% + 10px);
  padding: 0.25rem;
  margin: 0;
}

.tree-employers__column:not(:last-child) {
  border-right: 1px solid #ddd;
}

.tree-employers__row {
  border: unset;
  width: 100%;
}

.tree-employers__row:not(:first-child) {
  margin-left: 1rem;
  margin-right: 1rem;
}

</style>
