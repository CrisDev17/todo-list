<template>
  <div class="container" style="max-width: 600px">
    <!-- Heading -->
    <h2 class="text-center mt-5">Danh sách công việc</h2>

    <!-- Input -->
    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="task"
        placeholder="Điền công việc"
        class="w-100 form-control"
      />
      <button class="btn btn-warning rounded-0" @click="submitTask">
        Tạo
      </button>
    </div>

    <!-- Task table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Công việc</th>
          <th scope="col" style="width: 120px">Trạng thái</th>
          <th scope="col" class="text-center">Xóa</th>
          <th scope="col" class="text-center">Sửa</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ 'line-through': task.status === 'Đã xong' }">
              {{ task.name }}
            </span>
          </td>
          <td>
            <span
              class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'Mới tạo',
                'text-success': task.status === 'Đã xong',
                'text-warning': task.status === 'Đang làm',
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
            </span>
          </td>
          <td class="text-center">
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
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["Mới tạo", "Đang làm", "Đã xong"],

      /* Status could be: 'to-do' / 'in-progress' / 'finished' */
      tasks: [
        {
          name: "Đi học",
          status: "Đang làm",
        },
        {
          name: "Tập thể dục",
          status: "Đã xong",
        },
        {
          name: "Ngồi code",
          status: "Đang làm",
        },
      ],
    };
  },

  methods: {

    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },


    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },


    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },


    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
     
        this.tasks.push({
          name: this.task,
          status: "Mới tạo",
        });
      }

      this.task = "";
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none; 
  -webkit-user-select: none; 
  -khtml-user-select: none; 
  -moz-user-select: none;
  -ms-user-select: none; 
  user-select: none; 
}                                 
.line-through {
  text-decoration: line-through;
}
</style>