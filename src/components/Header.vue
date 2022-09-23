<template>
  <v-card flat>
    <v-toolbar color="primary" dark extended flat> </v-toolbar>

    <v-card class="mx-auto" max-width="800" style="margin-top: -64px">
      <v-toolbar flat>
        <v-toolbar-title class="grey--text justify-center"> IRAPARDAZ </v-toolbar-title>
      </v-toolbar>

      <v-divider></v-divider>

      <v-card-text>
        <v-main>
          <slot></slot>
        </v-main>
      </v-card-text>
    </v-card>
  </v-card>
</template>

<script>
export default {};
</script>
