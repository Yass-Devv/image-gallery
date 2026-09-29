<script setup>
import { ref, computed } from 'vue'

// 1. Déclarations réactives
const newImageUrl = ref('')
const images = ref([])
const errorMessage = ref('')

// 2. Propriété calculée (computed) pour le total des images
const totalImages = computed(() => images.value.length)

// 3. Validation basique de l'URL
function isValidUrl(string) {
  try {
    const url = new URL(string)
    return url.protocol === 'http:' || url.protocol === 'https:'
  } catch {
    return false
  }
}

// 4. Fonctions d'action
function handleAddImage() {
  const url = newImageUrl.value.trim()

  // Réinitialiser le message d'erreur à chaque tentative
  errorMessage.value = ''

  // Contrainte : empêcher l'ajout d'URL vides ou non valides
  if (!url) {
    errorMessage.value = "Le champ d'URL ne peut pas être vide."
    return
  }

  if (!isValidUrl(url)) {
    errorMessage.value = "Veuillez entrer une URL valide commençant par http:// ou https://."
    return
  }

  // Ajout de l'image (avec un id unique pour sécuriser le :key ps: ici j'utilise date now car c'est toujours unique )
  images.value.push({
    id: Date.now(),
    url: url
  })

  // Réinitialisation du champ de saisie pour la sécurité
  newImageUrl.value = ''
}

function handleRemoveImage(index) {
  images.value.splice(index, 1)
}
</script>



<template>
  <main class="app-container">
    <h1>Galerie d'images dynamique</h1>

    <!-- 1. Formulaire d'ajout avec v-model et @submit -->
    <form class="input-section" @submit.prevent="handleAddImage">
      <input
        v-model="newImageUrl"
        type="text"
        placeholder="Entrez l'URL d'une image..."
        aria-label="URL de l'image"
      />
      <button type="submit">Ajouter</button>
    </form>

    <!-- Message d'erreur de validation -->
    <p v-if="errorMessage" class="error-text">
      {{ errorMessage }}
    </p>

    <!-- 2. Affichage dynamique du total via la computed property -->
    <section class="counter-section">
      <p><strong>Total d'images :</strong> {{ totalImages }}</p>
    </section>

    <!-- 3. Condition v-if / v-else pour l'état vide ou la liste -->
    <section class="gallery-section">
      <p v-if="images.length === 0" class="empty-state">
        Aucune image ajoutée
      </p>

      <div v-else class="gallery">
        <!-- Rendu dynamique avec v-for -->
        <article
          v-for="(item, index) in images"
          :key="item.id"
          class="card"
        >
          <img :src="item.url" :alt="'Image ' + (index + 1)" />
          <!-- Événement de suppression avec v-on (@click) -->
          <button class="delete-btn" @click="handleRemoveImage(index)">
            Supprimer
          </button>
        </article>
      </div>
    </section>
  </main>
</template>




<style scoped>
.app-container {
  max-width: 800px;
  margin: 40px auto;
  padding: 0 20px;
  font-family: Arial, sans-serif;
  color: #191818;
}

h1 {
  text-align: center;
  margin-bottom: 25px;
}

.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 12px;
}

.input-section input {
  flex: 1;
  padding: 10px 14px;
  font-size: 15px;
  border: 1px solid #ccc;
  border-radius: 6px;
}

.input-section button {
  padding: 10px 20px;
  font-size: 15px;
  font-weight: bold;
  background-color: #42b883;
  color: #fff;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.input-section button:hover {
  background-color: #33a06f;
}

.error-text {
  color: #d9383a;
  margin: 0 0 16px 0;
  font-size: 14px;
}

.counter-section {
  font-size: 17px;
  margin-bottom: 20px;
}

.empty-state {
  font-style: italic;
  color: #666;
  background: #f9f9f9;
  padding: 24px;
  text-align: center;
  border-radius: 6px;
  border: 1px dashed #ddd;
}

.gallery {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.card {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #fff;
  border: 1px solid #eee;
  border-radius: 8px;
  padding: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
}

.card img {
  width: 100%;
  height: 140px;
  object-fit: cover;
  border-radius: 4px;
  margin-bottom: 10px;
}

.delete-btn {
  width: 100%;
  padding: 8px 12px;
  background-color: #e02424;
  color: white;
  
  border-radius: 4px;
  font-size: 15px;
}

.delete-btn:hover {
  background-color: #bc1919;
}
</style>


