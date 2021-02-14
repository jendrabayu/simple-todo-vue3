<template>
  <div>
    <button
      :class="filter === 'all' ? 'active' : ''"
      @click="filterChanged('all')"
    >
      All
    </button>
    <button
      :class="filter === 'active' ? 'active' : ''"
      @click="filterChanged('active')"
    >
      Active
    </button>
    <button
      :class="filter === 'completed' ? 'active' : ''"
      @click="filterChanged('completed')"
    >
      Completed
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      filter: "all",
    };
  },
  methods: {
    filterChanged(filter) {
      this.filter = filter;
      this.$emit("filterChanged", this.filter);
    },
  },
};
</script>