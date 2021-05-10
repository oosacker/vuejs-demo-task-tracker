<template>
  <footer>
    <p>Copyright &copy; 2021</p>
    <router-link to="/about">About</router-link>
    <!-- don't use <a href> as it will refresh the page -->
  </footer>
</template>

<style scoped>
footer {
  color: #333;
  text-align: center;
  padding: 20px 0 0 20px;
}
</style>
