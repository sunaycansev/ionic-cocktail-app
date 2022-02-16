<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-back-button></ion-back-button>
        </ion-buttons>
        <ion-title>{{ ingredient }} Drinks</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="loading">
      <div class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </div>
    </ion-content>

    <ion-content :fullscreen="true" v-else>
      <ion-list>
        <ion-item
          v-for="drink in lstDrinks"
          :key="drink.idDrink"
          @click="goToDrink(drink.idDrink)"
        >
          <ion-avatar slot="start">
            <img :src="drink.strDrinkThumb" />
          </ion-avatar>
          <ion-label>
            <h2>{{ drink.strDrink }}</h2>
          </ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup>
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonAvatar,
  IonLabel,
  IonSpinner,
  IonItem,
  IonList,
  IonBackButton,
  IonButtons
} from '@ionic/vue'
import { ref } from 'vue'
import axios from 'axios'
import { useRouter, useRoute } from 'vue-router'

const lstDrinks = ref([])
const loading = ref(false)
const router = useRouter()
const route = useRoute()
const ingredient = route.params.ingredient

const fetchDrinksByIngredient = async () => {
  loading.value = true

  const res = await axios.get(
    `https://www.thecocktaildb.com/api/json/v1/1/filter.php?i=${ingredient}`
  )

  if (res.data) {
    lstDrinks.value = res.data?.drinks
  }
  loading.value = false
}

const goToDrink = (ingredient) => {
  router.push(`/drinks-by-ingredient/${ingredient.strIngredient1}`)
}

fetchDrinksByIngredient(ingredient)
</script>

<style scoped>
.loading-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

ion-spinner {
  transform: scale(1.5);
}
</style>
