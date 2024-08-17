<template>
  <div class="flex container flex-col w-full gap-5">
    <div class="bg-green-300 relative mt-20 rounded-lg p-5">
      <h1 class="text-5xl text-orange-500 text-center font-bold">
        TodoLisTApp FOR Mahdavi
      </h1>
      <div class="w-full">
        <div class="flex gap-5 items-center p-2 w-full" id="Buttons">
          <label class="w-fit font-bold" for="TodoInput"
            >Add your new work :</label
          >
          <input
            v-model="TodoObject"
            class="w-3/4 h-12 indent-3 focus:outline-none rounded-lg"
            type="text"
            name="TodoInput"
            id="TodoInput"
          />
        </div>
        <div class="flex gap-5 p-2" id="Buttons">
          <button
            @click="AddTodo()"
            id="add"
            ref="add"
            class="w-52 h-12 border-none rounded-xl bg-green-600 text-white hover:bg-slate-200 hover:text-green-600 transition duration-700"
          >
            ADD
          </button>
          <button
            @click="RemoveAll()"
            id="ramoveAll"
            class="w-52 h-12 rounded-xl bg-red-600 text-white hover:bg-slate-200 hover:text-red-600 transition duration-700"
          >
            Remove All Todo Objecct
          </button>
        </div>
      </div>
    </div>

    <div id="TodoBox" class="container flex flex-col gap-3">
      <div
        v-for="(item, index) in TodoArray"
        :key="index"
        class="flex flex-row items-center p-5 border-2 bg-slate-300 border-black rounded-lg"
        id="todoObject"
      >
        <div class="w-full">
          <input
            class="w-full bg-transparent rounded-lg"
            type="text"
            name="item"
            id="item"
            disabled
          />
          {{ item.Name }}
        </div>
        <div id="ButtonsBox " class="flex gap-2 p-2">
          <button
            @click="editTodo(index)"
            id="editBox"
            class="h-12 w-24 border-none rounded-xl bg-green-600 text-white hover:bg-slate-200 hover:text-green-600 transition duration-700"
          >
            <span class="fa fa-pen"></span>
          </button>
          <button
            @click="removeTodo(index)"
            id="ramoveBox"
            class="h-12 w-24 rounded-xl bg-red-600 text-white hover:bg-slate-200 hover:text-red-600 transition duration-700"
          >
            <span class="fa fa-trash"></span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

 <script >
import { ref } from "vue";
export default {
  data() {
    return {
      name: "TodoList",
      TodoObject: "",
      EditTodo: null,
      TodoArray: [],
    };
  },
  created() {
    this.load();
  },
  methods: {
    AddTodo() {
      if (this.TodoObject.length === 0) {
        confirm("یجیزی وارد کن");
        return;
      }
      if (this.EditTodo === null) {
        this.TodoArray.push({
          Name: this.TodoObject,
          status: "todo",
        });
      } else {
        document.querySelector("#add").innerHTML = "Add";
        this.TodoArray[this.EditTodo].Name = this.TodoObject;
        this.EditTodo = null;
      }
      this.TodoObject = "";
      this.setLocalStorge(this.TodoArray);
    },

    removeTodo(index) {
      this.TodoArray.splice(index, 1);
      this.setLocalStorge(this.TodoArray);
    },
    RemoveAll() {
      for (let i = 0; i <= this.TodoArray.length + 1; i++) {
        this.TodoArray.pop(i);
      }
      this.setLocalStorge(this.TodoArray);
    },
    editTodo(index) {
      this.TodoObject = this.TodoArray[index].Name;
      this.EditTodo = index;
      document.querySelector("#add").innerHTML = "Save";
      this.setLocalStorge(this.TodoArray);
    },
    setLocalStorge(array) {
      localStorage.setItem("todo", JSON.stringify(array));
    },
    getLocalstorge() {
      let res = JSON.parse(localStorage.getItem("todo"));
      return res;
    },
    load() {
      this.TodoArray = this.getLocalstorge();
    },
  },
};
</script>
<style scoped>
</style>
