<template>
  <div>
    <label>
      <input
        type="checkbox"
        @change="checkAllChanged"
        :checked="!anyRemaining"
        :disabled="!anyTodos"
      />Check All
    </label>
  </div>
</template>

<script>
export default {
  props: {
    anyTodos: {
      type: Boolean,
      required: true,
    },
    anyRemaining: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    checkAllChanged(event) {
      this.$emit("checkAllTodo", event);
    },
  },
};
</script>