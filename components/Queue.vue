<template>
  <div>
    <h2 class="mt-6">
      {{ title }} ({{ processList.length }})
    </h2>
    <v-row v-if="processList.length > 0">
      <v-col
        v-for="p in processList.slice(0,11)"
        :key="p.id"
        cols="2"
      >
        <v-tooltip bottom color="blue-grey darken-1">
          <template #activator="{ on, attrs }">
            <v-card
              class="pa-2"
              tile
              outlined
              v-bind="attrs"
              v-on="on"
            >
              P{{ p.id }}
            </v-card>
          </template>
          <span>Tp: {{ p.burstTime }}</span><br>
          <span>Tw: {{ p.waitTime }}</span>
        </v-tooltip>
      </v-col>
      <v-col v-if="processList.length > 11" cols="2">
        <p>...</p>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  props: {
    title: { type: String, required: true },
    processList: { type: Array, required: true }
  }
}
</script>

<style>

</style>
