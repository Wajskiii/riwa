<template>
  <q-page class="flex flex-center">
    <q-card class="my-card">
      <q-card-section>
        <div class="text-h6">Random stranica(Random kid)</div>
      </q-card-section>

      <q-card-section>
        {{ poruka }}
      </q-card-section>

      <q-card-actions align="left">
        <q-btn
          color="pink"
          label="click me.....mnh"
          @click="showMessage"
        />
      </q-card-actions>
    </q-card>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'

const poruka = ref('Ovo je tekst prvi moj kju kard')

function showMessage() {
  poruka.value = 'Kliknuo si me >.<'
}
</script>

<style scoped>
.my-card {
  width: 100%;
  max-width: 400px;
}
</style>