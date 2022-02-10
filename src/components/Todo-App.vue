<template>
  <div class="container">
    <h2 class="text-center mt-5">ToDO List</h2>

    <!-- Input -->
    <div class="d-flex">
      <input
        v-model="task"
        type="text"
        placeholder="Enter task"
        class="form-control"
      />
      <button @click="submitTask" class="btn btn-warning rounded-0">
        Print
      </button>
    </div>

    <!-- Task table -->
    <table class="table table-bordered mt-2">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td :class="{'finished': task.status === 'finished'}">{{ task.name }}</td>
          <td style="width: 120px" class="pointer" @click="changeStatus(index)">
            <span :class="{'to-do': task.status === 'to-do',
            'in-progress': task.status === 'in-progress',
            'done': task.status === 'finished'}">{{ firstCharUpper(task.status) }}</span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      task: "",
      editedTask: null,
      availableStatuses: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "Wake up Thuowng",
          status: "to-do",
        },
        {
          name: "Take 200.000 VND",
          status: "in-progress",
        },
        {
          name: "Kiss her",
          status: "finished",
        },
      ],
    };
  },

  methods: {
    submitTask() {
      if (this.task.length === 0) return;
      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do",
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = "";
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index) {
      let number = this.availableStatuses.indexOf(this.tasks[index].status);
      number++;
      this.tasks[index].status = this.availableStatuses[number % 3];
    },

    firstCharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  },
};
</script>
<style scoped>
.pointer {
  cursor: pointer;
  user-select: none;
}
.finished{
  text-decoration: line-through;
}
.to-do{
  color: red;
}
.in-progress{
  color: yellow;
}
.done{
  color: greenyellow;
}
</style>