<template>
  <div>
    <button
      style="margin-right: 0"
      :disabled="!anyCompleted"
      @click="clearCompleted"
    >
      Clear Complete
    </button>
  </div>
</template>

<script>
export default {
  props: {
    anyCompleted: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    clearCompleted() {
      this.$emit("clearCompleted");
    },
  },
};
</script>