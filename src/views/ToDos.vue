<template>
  <v-card max-width="960" elevation="0" class="mx-auto my-5">
    <template slot="progress">
      <v-progress-linear
        color="deep-purple"
        height="10"
        indeterminate
      ></v-progress-linear>
    </template>

    <v-card-title class="justify-center">
      <v-btn href="mailto: ashkansabbaghi73@gmail.com" class="ma-1" color="grey" plain>
        Click to contact me
      </v-btn>
    </v-card-title>

    <v-card-text>
      <v-row class="justify-center">
        <!-- inputs -->
        <v-col cols="12" sm="6" md="6">
          <v-text-field
            v-if="!isEdit"
            v-model.trim="newTodo"
            class="input"
            label="new task"
            elevation="0"
            solo
            dense
            @keyup.enter="addTodo"
          ></v-text-field>
          <v-text-field
            v-else
            v-model.trim="newTodo"
            class="input"
            label="enter edit"
            elevation="0"
            solo
            dense
            @keyup.enter="EditTodo"
          ></v-text-field>
        </v-col>
        <!-- actions inputs -->
        <v-col cols="12" md="6" sm="3">
          <v-btn v-if="!isEdit" @click.prevent="addTodo">
            add
            <v-icon right>mdi-plus</v-icon>
          </v-btn>
          <v-btn v-else @click.prevent="EditTodo">
            edit
            <v-icon right>mdi-pencil</v-icon>
          </v-btn>
        </v-col>
      </v-row>
    </v-card-text>

    <v-card-text>
      <!-- component -->
      <!-- <span>{{ task }}</span> -->
      <v-row>
        <Task
          :todos="tasks"
          :isTask="true"
          @activeEditTodo="activeEditTodo"
          @removeTodo="removeTodo"
          @taskToDone="taskToDone"
        />
        <Task
          :todos="tasksDone"
          :isTask="false"
          @activeEditTodo="activeEditTodo"
          @removeTodo="removeTodo"
          @doneToTask="doneToTask"
        />
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
import Task from "@/components/Task";
export default {
  name: "home",
  components: { Task },
  data: () => ({
    isEdit: false,
    newTodo: "",
    tasks: [],
    tasksDone: [],
    index: null,
    task: [],
  }),
  created() {
    this.tasks = this.getTask() || [];
    this.tasksDone = this.getTaskDone() || [];
  },
  methods: {
    getTask() {
      return JSON.parse(localStorage.getItem("tasks"));
    },
    getTaskDone() {
      return JSON.parse(localStorage.getItem("tasksDone"));
    },
    addTodo() {
      const val = this.newTodo;
      if (!val) return;
      this.saveTask(val);
      this.newTodo = "";
    },

    taskToDone(val, i) {
      this.tasks.splice(i, 1);
      this.saveTaskDone(val);
      this.setStorage(true, this.tasks);
    },
    doneToTask(val, i) {
      this.tasksDone.splice(i, 1);
      this.saveTask(val);
      this.setStorage(false, this.tasksDone);
    },

    activeEditTodo(t, i) {
      this.isEdit = true;
      this.newTodo = t;
      this.index = i;
    },

    EditTodo() {
      const val = this.newTodo;
      if (!val) return;
      this.tasks.splice(this.index, 1, val);
      this.setStorage(true, this.tasks);
      this.reset();
    },

    reset() {
      this.index = null;
      this.isEdit = false;
      this.newTodo = "";
    },

    removeTodo(i, isTask) {
      if (isTask) {
        this.tasks.splice(i, 1);
        this.setStorage(true, this.tasks);
      } else {
        this.tasksDone.splice(i, 1);
        this.setStorage(false, this.tasksDone);
      }
    },

    saveTask(val) {
      this.tasks.reverse();
      this.tasks.push(val);
      this.tasks.reverse();
      this.setStorage(true, this.tasks);
    },
    saveTaskDone(val) {
      this.tasksDone.reverse();
      this.tasksDone.push(val);
      this.tasksDone.reverse();
      this.setStorage(false, this.tasksDone);
    },

    setStorage(isTask, items) {
      isTask
        ? localStorage.setItem("tasks", JSON.stringify(items))
        : localStorage.setItem("tasksDone", JSON.stringify(items));
    },
  },
};
</script>

<style lang="scss" scope>
.input {
  .v-input__slot {
    box-shadow: none !important;
    background-color: rgb(246, 246, 246) !important;
  }
}
</style>
