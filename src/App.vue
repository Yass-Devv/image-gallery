<script setup>
import { ref, computed } from 'vue'

const newImageUrl = ref('')
const images = ref([])
const errorMessage = ref('')

const totalImages = computed(() => images.value.length)

function isValidUrl(string) {
  try {
    const url = new URL(string)
    return url.protocol === 'http:' || url.protocol === 'https:'
  } catch {
    return false
  }
}

function handleAddImage() {
  const url = newImageUrl.value.trim()
  errorMessage.value = ''

  if (!url) {
    errorMessage.value = "L'URL ne peut pas être vide."
    return
  }

  if (!isValidUrl(url)) {
    errorMessage.value = "L'URL est invalide (doit commencer par http:// ou https:// donc fais une url valide et juste )."
    return
  }

  images.value.push({
    id: Date.now(),
    url: url
  })

  newImageUrl.value = ''
}

function handleRemoveImage(index) {
  images.value.splice(index, 1)
}
</script>

<template>
  <div>
    <h1>Galerie d'images</h1>

    <!-- Formulaire d'ajout -->
    <div>
      <input
        v-model="newImageUrl"
        type="text"
        placeholder="Entrez l'URL d'une image"
      />
      <button @click="handleAddImage">Ajouter</button>
    </div>

    <!-- Message d'erreur -->
    <p v-if="errorMessage" style="color: red;">
      {{ errorMessage }}
    </p>

    <!-- Total d'images -->
    <p>Total d'images : {{ totalImages }}</p>

    <!-- Message si galerie vide -->
    <p v-if="images.length === 0">
      Aucune image ajoutée
    </p>

    <!-- Liste des images -->
    <div v-else class="gallery">
      <div v-for="(item, index) in images" :key="item.id" class="card">
        <img :src="item.url" :alt="'Image ' + (index + 1)" />
        <button @click="handleRemoveImage(index)">Supprimer</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Strict minimum pour éviter que les images n'explosent l'écran */
.gallery {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
}

.card {
  display: flex;
  flex-direction: column;
  width: 150px;
}

img {
  width: 150px;
  height: 150px;
  object-fit: cover;
  margin-bottom: 5px;
}
</style>