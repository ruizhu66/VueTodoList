<template>
  <header>
    <input type="text" placeholder="enter todos, press enter to confirm" @keyup.enter="add">
  </header>
</template>

<script>
import {nanoid} from "nanoid";

export default {
  name: "Top",
  // data() {
  //   return {
  //     title: ""
  //   }
  // v-model="title"
  // },
  methods: {
    add(e) {
      if(e.target.value !== "") {
        const todoObj = {id: nanoid(), title: e.target.value, done: false}
        this.$bus.$emit("addTodo", todoObj);
        e.target.value = "";
      } else {
        alert("do not input empty todoItem")
      }
    }
  }
}
</script>

<style scoped>
  header {
    margin-bottom: 10px;
  }
  header > input {
    width: 585px;
    height: 20px;
    padding: 10px 5px;
  }

  header > input:focus {
    outline: none;
    border-style:solid;
    border-color: #fff;
    box-shadow: 0 0 15px #03a9f4;
  }
</style>