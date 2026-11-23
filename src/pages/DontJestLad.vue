<template>
  <q-page class="flex flex-center column q-gutter-md q-pa-md">
    <!-- joke lad! -->
    <q-card class="my-card">
      <q-card-section>
        <div class="text-h6">Don't jest lad</div>
      </q-card-section>

      <q-card-section>
        <q-spinner v-if="loading" color="pink" size="2em" />
        <div v-else-if="error" class="text-negative">{{ error }}</div>
        <div v-else>{{ joke }}</div>
      </q-card-section>

      <q-card-actions align="left">
        <q-btn color="pink" label="Get joke" :disable="loading" @click="getJoke" />
      </q-card-actions>
    </q-card>

    <!-- bored api -->
    <q-card class="my-card">
      <q-card-section>
        <div class="text-h6">ti dosadno aaa?</div>
      </q-card-section>

      <q-card-section>
        <q-spinner v-if="boredLoading" color="purple" size="2em" />
        <div v-else-if="boredError" class="text-negative">{{ boredError }}</div>
        <div v-else-if="bored">
          <div class="text-subtitle1">{{ bored.activity }}</div>
          <div>Tip: {{ bored.type }}</div>
          <div>Broj sudionika: {{ bored.participants }}</div>
          <div>Cijena: {{ bored.price }}</div>
        </div>
      </q-card-section>

      <q-card-actions align="left">
        <q-btn
          color="primary"
          label="Nova aktivnost"
          :disable="boredLoading"
          @click="loadBored"
        />
      </q-card-actions>
    </q-card>

    <!-- cat fact -->
    <q-card class="my-card">
      <q-card-section>
        <div class="text-h6">Činjenica o mačkama</div>
      </q-card-section>

      <q-card-section>
        <q-spinner v-if="catLoading" color="orange" size="2em" />
        <div v-else-if="catError" class="text-negative">{{ catError }}</div>
        <div v-else-if="cat">
          <div class="text-subtitle1">{{ cat.fact }}</div>
          <div class="text-caption q-mt-sm">Duljina: {{ cat.length }} znakova</div>
        </div>
      </q-card-section>

      <q-card-actions align="left">
        <q-btn
          color="orange"
          label="Nova faktualna"
          :disable="catLoading"
          @click="loadCat"
        />
      </q-card-actions>
    </q-card>
  </q-page>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

// joke
const joke = ref('')
const loading = ref(false)
const error = ref('')

async function getJoke() {
  loading.value = true
  error.value = ''
  try {
    const response = await axios.get('https://dumbapis.com/joke')
    const data = response.data
    joke.value = typeof data === 'string' ? data : data.joke ?? JSON.stringify(data)
  } catch (e) {
    error.value = 'Failed to load joke: ' + e.message
  } finally {
    loading.value = false
  }
}

// bored api
const bored = ref(null)
const boredLoading = ref(false)
const boredError = ref('')

async function loadBored() {
  boredLoading.value = true
  boredError.value = ''
  try {
    const response = await axios.get('/api-bored/random')
    console.log(response.data)
    bored.value = response.data
  } catch (e) {
    boredError.value = 'Greška pri dohvaćanju: ' + e.message
  } finally {
    boredLoading.value = false
  }
}

// cat fact
const cat = ref(null)
const catLoading = ref(false)
const catError = ref('')

async function loadCat() {
  catLoading.value = true
  catError.value = ''
  try {
    const response = await axios.get('https://catfact.ninja/fact')
    cat.value = response.data
  } catch (e) {
    catError.value = 'Greška pri dohvaćanju: ' + e.message
  } finally {
    catLoading.value = false
  }
}

onMounted(() => {
  getJoke()
  loadBored()
  loadCat()
})
</script>

<style scoped>
.my-card {
  width: 100%;
  max-width: 400px;
}
</style>