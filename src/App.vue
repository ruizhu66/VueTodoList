<template>
  <div id="app">
    <Top></Top>
    <List :todos="todos"></List>
    <Bot :todos="todos" ></Bot>
  </div>
</template>

<script>
import Top from "./components/Top";
import List from "./components/List";
import Bot from "./components/Bot";
import pubsub from "pubsub-js";


export default {
  name: 'App',
  components: {
    Top,
    List,
    Bot
  },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem("todos")) || []
    }
  },
  mounted() {
    //全局事件总线
    this.$bus.$on("addTodo", (todoObj)=>{
      this.todos.unshift(todoObj);
    })

    this.$bus.$on("checkAllTodo", (done)=>{
      this.todos.forEach(item=>{
        item.done = done;
      })
    })

    this.$bus.$on("clearAllTodo", ()=>{
      this.todos = this.todos.filter(item=>{
        return !item.done;
      })
    })

    this.$bus.$on("checkTodo", (id)=>{
      this.todos.forEach(item=>{
        if(item.id === id) {
          item.done = !item.done
        }
      })
    })

    // this.$bus.$on("checkDelete", (id)=>{
    //   this.todos = this.todos.filter(item=>{
    //     return item.id !== id;
    //   })
    // })

    //消息订阅走一波，做一个函数即可
    this.pubId = pubsub.subscribe("checkDelete", (msgName, id)=>{
        this.todos = this.todos.filter(item=>{
          return item.id !== id;
        })
    })

    //更新
    this.$bus.$on("updateTodo", (value, id)=>{
      this.todos.forEach(item=>{
        if(item.id === id) {
          item.title = value;
        }
      })
    })
  },
  beforeDestroy() {
    this.$bus.$off(["addTodo", "checkAllTodo", "clearAllTodo", "checkTodo", "updateTodo"]);
    //取消订阅
    pubsub.unsubscribe(this.pubId);
  },
  watch: {
    //set everything into local, when refresh we can see it
    todos: {
      deep: true,
      handler(value) {
        localStorage.setItem("todos", JSON.stringify(value))
      }
    }
  }
}
</script>

<style>
 #app {
   width: 600px;
   margin: 10px auto;
   border: 1px solid gray;
   padding: 10px;
   border-radius: 5px;
 }

 input[type="checkbox"] {
   vertical-align: middle;
 }
 ul {
   padding: 0;
   margin: 0;
 }
 ul li {
   list-style: none;
 }

 .btn {
   color: #fff;
   border-radius: 5px;
   background-color: red;
   padding: 3px 10px;
   border: none;
   font-weight: bold;
   cursor: pointer;
 }

 .btn-edit {
   color: #fff;
   border-radius: 5px;
   background-color: skyblue;
   padding: 3px 10px;
   border: none;
   font-weight: bold;
   cursor: pointer;
   margin-right: 10px;
 }

</style>
