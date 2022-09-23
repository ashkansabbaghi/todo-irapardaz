<template>
  <v-col>
    <v-card-title v-if="isTask">Remaining Task :</v-card-title>
    <v-card-title v-else>Done :</v-card-title>
    <!-- <v-list-item-group> -->
    <v-slide-x-transition class="py-0" group tag="v-container">
      <v-row class="item" v-for="(todo, i) in todos" :key="i">
        <template>
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
        </template>
      </v-row>
    </v-slide-x-transition>
    <!-- </v-list-item-group> -->
  </v-col>
</template>

<script>
export default {
  props: ["todos", "isTask"],
};
</script>

<style lang="scss" scoped>
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
