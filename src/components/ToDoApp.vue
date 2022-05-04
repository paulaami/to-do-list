<template>
  <div class="container">
    <h2 class="text-center mt-5">Vue To Do App</h2>
    <!-- Input -->
    <div class="d-flex">
      <input
        v-model="task"
        type="text"
        placeholder="Enter taska"
        class="form-control"
      />
      <button @click="submitTask" class="btn btn-warning rounded-0">
        SUBMIT
      </button>
    </div>
    <!-- Table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th class="text-center" scope="col">#</th>
          <th class="text-center" scope="col">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ finished: task.status === 'finished' }">{{
              task.name
            }}</span>
          </td>
          <td>
            <span
              @click="changeStatus(index)"
              :class="[
                'pointer',
                {
                  'text-danger': task.status === 'to-do',
                  'text-success': task.status === 'in-progress',
                },
              ]"
              >{{ firstCharUpper(task.status) }}</span
            >
          </td>
          <td>
            <div @click="editTask(index)" class="text-center">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div @click="deleteTask(index)" class="text-center">
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
  name: "ToDoApp",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      availableStatuses: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "Steal bananas",
          status: "to-do",
        },
        {
          name: "Eat choclate",
          status: "in-progress",
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
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
  text-transform: uppercase;
}
</style>
