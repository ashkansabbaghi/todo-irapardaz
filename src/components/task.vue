<template>
  <v-col>
    <v-card-title>
      <span class="text--disabled" v-text="isTask ? 'Remaining Task :' : 'Done :'"></span>
      <span class="ml-2" v-if="todos.length > 0">{{ todos.length }}</span>
    </v-card-title>

    <v-slide-x-transition group name="list" tag="ul">
      <!--  replace key todo -> todo.id -->
      <v-row class="item" v-for="(todo, i) in todos" :key="todo">
        <v-col cols="7">
          <v-list-item>
            <template v-slot:default="{}">
              <v-list-item-action>
                <v-icon v-if="isTask" @click="$emit('taskToDone', todo, i)">
                  mdi-checkbox-multiple-blank-outline
                </v-icon>

                <v-icon
                  color="green darken-2"
                  v-else
                  @click="$emit('doneToTask', todo, i)"
                >
                  mdi-checkbox-multiple-marked-outline
                </v-icon>
              </v-list-item-action>

              <v-list-item-content>
                <v-list-item-title :class="{ 'text-decoration-line-through': !isTask }">
                  {{ todo }}
                </v-list-item-title>
              </v-list-item-content>
            </template>
          </v-list-item>
        </v-col>
        <!-- actions -->
        <v-col class="d-flex align-center" cols="12" sm="3">
          <!-- btn edit -->
          <v-btn v-if="isTask" icon @click.prevent="$emit('activeEditTodo', todo, i)">
            <v-icon>mdi-pencil</v-icon>
          </v-btn>
          <!-- btn close -->
          <v-btn icon color="red" @click.prevent="$emit('removeTodo', i, isTask)">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-col>
      </v-row>
    </v-slide-x-transition>
  </v-col>
</template>

<script>
export default {
  props: ["todos", "isTask"],
};
</script>

<style lang="scss">
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: scale(0.9);
}
.list-leave-to {
  transition: all 0.2s ease;
}
.list-enter-active,
.list-leave-active {
  transition: all 0.2s ease;
}
</style>
