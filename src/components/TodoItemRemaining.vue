<template>
  <div>{{ remaining }} Item Left</div>
</template>

<script>
export default {
  props: {
    remaining: {
      type: Number,
      required: true,
    },
  },
};
</script>