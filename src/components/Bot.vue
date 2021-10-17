<template>
  <footer class="clearfix" v-show="total">
    <div class="fl">
      <label>
        <input type="checkbox" name="checkall" id="checkall" :checked="isAll" @change="checkAll">check &nbsp;&nbsp;
      </label>
      <span>Completed {{dontTotal}} / All {{total}}</span>
    </div>

    <div class="fr">
      <button class="clearall" @click="clearAll">Delete completed todo</button>
    </div>
  </footer>
</template>

<script>
export default {
  name: "Bot",
  props: ["todos"],
  computed: {
    dontTotal() {
      //1st solving
      // let count = 0;
      // this.todos.map(item=>{
      //   if(item.done) {
      //     count++;
      //   }
      // })
      // return count;

      return this.todos.reduce((prev, curr)=>{
        return prev + (curr.done ? 1 : 0);
      }, 0)
    },
    total() {
      return this.todos.length;
    },
    isAll() {
      // if(this.total > 0) {
      //   return this.total === this.dontTotal;
      // } else {
      //   return false;
      // }
      return this.total === this.dontTotal && this.total > 0;
    }
  },
  methods: {
    checkAll(e) {
      this.$bus.$emit("checkAllTodo", e.target.checked)
      // this.checkAllTodo(e.target.checked)
    },
    clearAll() {
      if(this.dontTotal > 0) {
        if(confirm("are you sure you want delete all completed?")) {
          this.$bus.$emit("clearAllTodo")
          // this.clearAllTodo();
        }
      }
    }
  }
}
</script>

<style scoped>
  .fl {
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

  .clearall {
    color: #fff;
    border-radius: 5px;
    background-color: red;
    padding: 3px 10px;
    border: none;
    font-weight: bold;
    cursor: pointer;
  }
</style>