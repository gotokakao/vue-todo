<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodoItem="addOneItem"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" 
      v-on:removeItem="removeOneItem" 
      v-on:toggleItem="toggleOneItem">
    </TodoList>
    <TodoFooter v-on:clearAll="clearAllItem"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from "./components/TodoHeader.vue";
import TodoFooter from "./components/TodoFooter.vue";
import TodoList from "./components/TodoList.vue";
import TodoInput from "./components/TodoInput.vue";


export default {
    data:function(){
    return{
      todoItems: []
    }
  },
  created:function(){
    if(localStorage.length > 0){
      for(let i=0;i < localStorage.length; i++){
        if(localStorage.key(i) !== "loglevel:webpack-dev-server"){
          this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))))
        }
      }
    }
  },
  methods:{
    addOneItem:function(todoItem){
        var obj = {completed:false, item: todoItem};
        localStorage.setItem(todoItem, JSON.stringify(obj));
        this.todoItems.push(obj);
    },
    removeOneItem:function(todoItem, index){
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(index, 1);
    },
    toggleOneItem:function(todoItem, index){
      // todoItem.completed = !todoItem.completed;
      this.todoItems[index].completed = !this.todoItems[index].completed;
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem))
    },
    clearAllItem:function(){
      localStorage.clear();
      this.todoItems = [];
    }
  },
  components:{
    'TodoHeader':TodoHeader,
    'TodoFooter':TodoFooter,
    'TodoList':TodoList,
    'TodoInput':TodoInput,

  }
}
</script>

<style> 
  body {
    text-align: center;
    background-color: #F6F6F6;

  }
  input {
    border-style: groove;
    width: 200px;
  }
  button {
    border-style: groove;
  }

  .shadow{
    box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
  }
</style>