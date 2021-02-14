<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input
        type="checkbox"
        @change="doneEdit"
        :checked="completed"
        v-model="completed"
      />
      <div v-if="!editing" class="todo-item-label" @dblclick="editTodo">
        {{ title }}
      </div>
      <input
        v-else
        class="todo-item-edit"
        type="text"
        :value="title"
        @keyup.esc="cancelEdit"
        v-model="title"
        @keyup.enter="doneEdit"
        v-focus
        @blur="doneEdit"
      />
    </div>
    <div>
      <button @click="pluralize">plural</button>
      <span class="remove-item" @click="removeTodo">&times;</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    todo: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
    checkAll: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      id: this.todo.id,
      title: this.todo.title,
      completed: this.todo.completed,
      editing: this.todo.editing,
      beforeEditCache: "",
    };
  },
  methods: {
    editTodo() {
      this.beforeEditCache = this.title;
      this.editing = true;
    },
    cancelEdit() {
      this.editing = false;
      this.title = this.beforeEditCache;
    },
    doneEdit() {
      if (this.title.trim("").length === 0) {
        this.title = this.beforeEditCache;
      }

      this.editing = false;
      this.$emit("doneEdit", {
        index: this.index,
        todo: {
          id: this.id,
          title: this.title,
          completed: this.completed,
          editing: this.editing,
        },
      });
    },
    pluralize() {
      this.title += "s";
      this.doneEdit();
    },
    removeTodo() {
      this.$emit("removeTodo", this.index);
    },
  },
  directives: {
    focus: {
      mounted(el) {
        el.focus();
      },
    },
  },
  watch: {
    checkAll(newVal, oldVal) {
      this.checkAll
        ? (this.completed = true)
        : (this.completed = this.todo.completed);
    },
  },
};
</script>