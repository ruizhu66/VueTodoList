<template>
  <li class="itemStyle clearfix">
    <label>
      <input
          type="checkbox"
          name="todo"
          :checked="todoObj.done"
          @change="handleCheck(todoObj.id)"
      ><span v-show="!todoObj.isEdit">{{todoObj.title}}</span>
      <input
          v-show="todoObj.isEdit"
          type="text"
          :value="todoObj.title"
          @blur="handleBlur(todoObj,$event)"
          ref="change"
      >
    </label>
    <button class="btn fr" @click="handleDelete(todoObj.id)">Delete</button>
    <button v-show="!todoObj.isEdit" class="btn-edit fr" @click="handleEdit(todoObj)">Change</button>
  </li>
</template>

<script>
import pubsub from "pubsub-js";

export default {
  name: "Item",
  props: ["todoObj"],
  methods: {
    handleCheck(id) {
      //give id back to APP component
      this.$bus.$emit("checkTodo", id);
    },
    handleDelete(id) {
      //give id back to delete
      if(confirm("are you sure to delete?")) {
        // this.$bus.$emit("checkDelete", id); 全局事件总线

        //发布消息
        pubsub.publish("checkDelete", id);
      }
    },
    //edit
    handleEdit(todoObj) {
      if(Object.prototype.hasOwnProperty.call(todoObj, "isEdit")) {
        todoObj.isEdit = true;
      } else {
        this.$set(todoObj, "isEdit", true)
      }

      //第一种解决
      // setTimeout(()=>{
      //   this.$refs.change.focus()
      // }, 200)

      this.$nextTick(()=>{
        this.$refs.change.focus()
      })
    },
    handleBlur(todoObj, e) {
      todoObj.isEdit = false;
      if(!e.target.value.trim()) {
        return alert("cant edit to empty string")
      }
      this.$bus.$emit("updateTodo", e.target.value, todoObj.id)
    }
  }
}
</script>

<style scoped>
  .itemStyle {
    padding: 10px 5px;
    border-bottom: 1px solid gray;
  }

  .itemStyle label {
    float: left;
  }

  .fr {
    float: right;
  }


  .clearfix:after{
    content: "";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
  }

  .clearfix {
    /* 触发 hasLayout */
    *zoom: 1;
  }
  li button {
    display: none;
  }
  li:hover {
    background-color: #ddd;
  }

  li:hover button {
    display: block;
  }
</style>