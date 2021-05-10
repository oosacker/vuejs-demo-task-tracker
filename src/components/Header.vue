<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      v-show="homePage"
      :text="showAddTask ? 'Hide' : 'Add Task'"
      :color="showAddTask ? 'Red' : 'Green'"
      @btn-click="$emit('toggle-add-task')"
    />
  </header>
</template>

<script>
import Button from "./Button";

export default {
  name: "Header",
  components: {
    Button,
  },
  props: {
    title: {
      type: String,
      default: "HELLOOOOO",
    },
    showAddTask: Boolean,
  },
  methods: {
    isHomepage() {
      if (this.$route.path === "/") {
        return true;
      } else {
        return false;
      }
    },
  },
  computed: {
    // use v-show to decide to show button or not depending on path
    homePage() {
      if (this.$route.path === "/") {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
}
</style>
