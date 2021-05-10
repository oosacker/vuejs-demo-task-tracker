<template>
  <button :style="{ background: color }" @click="clickHandler" class="btn">
    {{ text }}
  </button>
</template>

<script>
export default {
  name: "Button",
  props: {
    text: String,
    color: String,
  },
  methods: {
    clickHandler() {
      console.log("clicked add task button");
      this.$emit("btn-click");
    },
  },
};
</script>

<style scoped></style>
