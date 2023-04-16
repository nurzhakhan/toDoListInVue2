<script setup>
import ToDoList from "@/components/ToDoList.vue";
</script>

<template>
  <div id="app">

    <ToDoList />

  </div>
</template>

<style scoped>
header {
  display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
