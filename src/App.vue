<template>
  <div class="py-10">
    <img alt="Vue logo" class="mb-4 mx-auto" src="./assets/logo.png" width="100" height="100"/>
    <h1 class="font-bold text-2xl italic text-center mb-8">
      Vue3でメモアプリ
    </h1>
    <a
      href="https://windicss.org/"
      class="bg-gradient-to-r from-green-400 to-blue-500 w-200px block mx-auto px-4 py-3 text-white text-center italic rounded cursor-default transition-all duration-400 hover:rounded-2xl"
    >
      Windi.CSS
    </a>
    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="task"
        placeholder="こちらに入力して下さい"
        class="w-100 form-control"
      />
      <button class="btn btn-warning rounded-0" @click="submitTask">
        登録
      </button>
      <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">内容</th>
          <th scope="col" style="width: 100px">状況</th>
          <th scope="col" class="text-center"></th>
          <th scope="col" class="text-center"></th>
        </tr>
      </thead>
  <!-- <tbody>水平方向<tr>表の行部分を指定<td>テーブルのセルを指定<span>特に意味無し -->
      <tbody> 
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}
            </span>
          </td>
          <td>
            <span
              class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'memo',
                'text-success': task.status === 'finished',
                'text-warning': task.status === 'in-progress'
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
            </span>
          </td>
          <td class="text-center">
            <button v-on:click="deleteTask(task.status)" type="button">Delete</button>
            <div @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
          <td class="text-center">
            <div @click="editTask(index)">
              <p class="fa fa-pen pointer"></p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["memo", "in-progress", "finished"],
      /* Status could be: 'memo' / 'in-progress' / 'finished' */
      tasks: [
        {
          name: "例: サンプル",
          status: "memo"
        },
        {
          name: "例: 進行中",
          status: "in-progress"
        },
        {
          name: "例: 削除",
          status: "finished"
        }
      ]
    };
  },
  methods: {
    /**
     * Capitalize first character
     */
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
    /**
     * Change status of task by index
     */
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },
    /**
     * Deletes task by index
     */
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    /**
     * Edit task
     */
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    /**
     * Add / Update task
     */
    submitTask() {
      if (this.task.length === 0) return;
      /* We need to update the task */
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        /* We need to add new task */
        this.tasks.push({
          name: this.task,
          status: "memo"
        });
      }
      this.task = "";
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
