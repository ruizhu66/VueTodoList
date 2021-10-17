<template>
  <ul class="list">
    <Item
        v-for="todo in todos"
        :key="todo.id"
        :todoObj="todo"
    >
    </Item>
  </ul>
</template>

<script>
import Item from "./Item";

export default {
  name: "List",
  components: {
    Item
  },
  props: ["todos"]
}
</script>

<style scoped>
  .list {
    width: 595px;
    border: 1px solid gray;
    border-bottom: none;
    margin-bottom: 20px;
  }

</style>