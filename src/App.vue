<script setup>
//import de ref et computed 
import { ref, computed } from 'vue'
//variables réactives
const newImageUrl = ref('')
const images = ref([])
const errorMessage = ref('')

const totalImages = computed(() => images.value.length) //calcul du nombre avec length pour éviter les doublons de .value++

function isValidUrl(string) {
  try {
    const url = new URL(string)
    return url.protocol === 'http:' || url.protocol === 'https:' //sécurité protocol
  } catch {
    return false
  }
}

//vider l'url a chaque foi pour eviter beug et faille sécurité
function handleAddImage() {
  const url = newImageUrl.value.trim()
  errorMessage.value = ''

  //test si url existe sinon message d'erreur 
  if (!url) {
    errorMessage.value = "L'URL ne peut pas être vide."
    return
  }

  if (!isValidUrl(url)) {
    errorMessage.value = "L'URL est invalide (doit commencer par http:// ou https:// donc fais une url valide et juste )."
    return
  }

  //on utilise date now pour avoir un id unique a 100% et éviter les beugs 
  images.value.push({
    id: Date.now(),
    url: url
  })

  newImageUrl.value = '' // on vide aprés étude de l'url 
}

//focntion supprimer ppour le bouton delete
function handleRemoveImage(index) {
  images.value.splice(index, 1)
}
</script>

<template>
  <h1 class="title">Galerie d'images</h1>
  <div class="gallery">

    <!-- Formulaire d'ajout -->
    <div class="form">
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
        <button @click="handleRemoveImage(index)" class="delete" >Supprimer</button>
      </div>
    </div>
  </div>

</template>

<style scoped>
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

.title{
  color: green;
  border: 2px solid rgb(0, 64, 6);
  border-radius: 50px;
  padding: 10px 15px;
  justify-self: center;
  font-size: 50px;
  background-color: antiquewhite;
}

.gallery{
  display: flex;
  flex-direction: column;
  gap: 50px;
  justify-self: center;
  margin-top: 150px;
  font-size: 20px;
  font-weight: bolder;
}

.form{
  display: flex;
  flex-direction: row;
  border-radius: 50px;
  gap: 20px;
}

button{
  border-radius: 50px;
  padding: 10px 15px;
  background-color: rgb(21, 141, 0);
  color: bisque;
  font-size: 15px;
  font-weight: bolder;

}

button .delete{
  border-radius: 50px;
  padding: 10px 15px;
  background-color: rgb(21, 141, 0);
  color: bisque;
  font-size: 15px;
  font-weight: bolder;

}

input{
  border: 50px;
  border-radius: 50px;
  padding: 20px 15px;
  background-color: antiquewhite;
}

template{
  background-color: antiquewhite;
}

</style>