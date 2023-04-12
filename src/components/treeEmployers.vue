<template>
  <div class="tree-employers">
    <div class="tree-employers__body">
      <div class="tree-employers__column">
        <div
          :class="['tree-employers__column-text', { 'tree-employers__column-text_parent' : computedHasChildren }]"
          :style="computedTranslateNestedUser"
          @click="toggleChildrenNodes($event)"
        >
          {{ user.name }}
        </div>
      </div>
      <div class="tree-employers__column">
        {{ user.phone }}
      </div>
    </div>
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
  },
  computed: {
    computedHasChildren () {
      return this.user.children.length > 0
    },
    computedTranslateNestedUser () {
      return this.user.parentID !== null ? `transform: translateX(calc(0.5rem + ${this.user.parentID}rem))` : null
    }
  },
  methods: {
    toggleChildrenNodes (event) {
      try {
        const target = event.target || event.currentTarget
        const findNestedRow = target.closest('.tree-employers__body').nextElementSibling || null
        const findNestedText = target.closest('.tree-employers__column-text') || null

        if (!findNestedRow) {
          this.sendErrorCode('Нет дочерних элементов')
        } else {
          findNestedRow.classList.toggle('tree-employers__row_active')
        }

        if (!findNestedText) {
          this.sendErrorCode('Нет дочерних элементов')
        } else {
          findNestedText.classList.toggle('tree-employers__column-text_hide')
        }
      } catch (error) {
        console.error(error)
      }
    },
    sendErrorCode (msg) {
      return new Error(msg)
    }
  }
}
</script>

<style scoped>
.tree-employers {
  flex-basis: 100%;
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
  width: 100%;
  display: none;
}

.tree-employers__body {
  border-top: 1px solid #ddd;
}

.tree-employers__body_active {

}

.tree-employers__body:hover {
  background-color: #f2f2f2;
}

.tree-employers__column-text {
  position: relative;
  display: inline-flex;
  cursor: pointer;
}

.tree-employers__column-text_parent {
  padding-left: 10px;
}
.tree-employers__column-text_parent::before {
  content: '+';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
}

.tree-employers__row_active {
  display: block;
}

.tree-employers__column-text_hide::before {
  content: '-';
}
</style>
