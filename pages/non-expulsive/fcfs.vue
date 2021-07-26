<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <h1 class="text-h3 text-center my-6">
        Primero en llegar primero en ejecutar (FCFS)
      </h1>
      <h2 class="mt-6">
        Listos
      </h2>
      <v-row>
        <v-col
          v-for="n in 4"
          :key="n"
        >
          <v-card
            class="pa-2"
            tile
            outlined
          >
            Process
          </v-card>
        </v-col>
      </v-row>
      <h2 class="mt-6">
        CPU
      </h2>
      <v-row>
        <v-col
          v-for="n in 4"
          :key="n"
        >
          <v-card
            class="pa-2"
            tile
            outlined
          >
            Process
          </v-card>
        </v-col>
      </v-row>
      <h2 class="mt-6">
        Terminados
      </h2>
      <v-row>
        <v-col
          v-for="n in 4"
          :key="n"
        >
          <v-card
            class="pa-2"
            tile
            outlined
          >
            Process
          </v-card>
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data () {
    return {}
  }
}
</script>

<style>

</style>
