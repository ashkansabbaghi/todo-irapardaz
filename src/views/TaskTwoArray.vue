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
  }),
  methods: {
    addTodo() {
      const val = this.newTodo;
      if (!val) return;
      // console.log(val);
      this.tasks.push(val);
      this.tasks.reverse();
      this.newTodo = "";
    },

    taskToDone(val, i) {
      // console.log("taskToDone", i);
      this.tasks.splice(i, 1);
      this.tasksDone.push(val);
      this.tasksDone.reverse();
    },
    doneToTask(val, i) {
      // console.log("doneToTask", i);
      this.tasksDone.splice(i, 1);
      this.tasks.push(val);
      this.tasks.reverse();
    },

    activeEditTodo(t, i) {
      // console.log(t, i);
      this.isEdit = true;
      this.newTodo = t;
      this.index = i;
    },

    EditTodo() {
      const val = this.newTodo;
      // console.log(val, this.index);
      if (!val) return;
      this.tasks.splice(this.index, 1, val);
      console.log(this.tasks);
      this.reset();
    },

    reset() {
      this.index = null;
      this.isEdit = false;
      this.newTodo = "";
    },

    removeTodo(i, isTask) {
      // console.log(i, isTask);
      isTask ? this.tasks.splice(i, 1) : this.tasksDone.splice(i, 1);
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
