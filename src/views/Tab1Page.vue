<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Tab 1</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true" v-if="loading">
      <div class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-refresher slot="fixed" @ionRefresh="doRefresh">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>
      <ion-card>
        <img :src="randomCocktail.strDrinkThumb" alt="x" />
        <ion-card-header>
          <ion-card-subtitle>
            {{ randomCocktail.strCategory }} | Served In
            {{ randomCocktail.strGlass }}
          </ion-card-subtitle>
          <ion-card-title>
            {{ randomCocktail.strDrink }}
          </ion-card-title>
          <ion-card-content>
            <p>{{ randomCocktail.strInstructions }}</p>
            <ion-list-header> Ingredients </ion-list-header>
            <ion-list>
              <ion-item v-if="randomCocktail.strIngredient1">
                <ion-label>
                  <span v-if="randomCocktail.strMeasure1"
                    >{{ randomCocktail.strMeasure1 }} -
                  </span>
                  {{ randomCocktail.strIngredient1 }}
                </ion-label>
              </ion-item>
            </ion-list>
          </ion-card-content>
        </ion-card-header>
      </ion-card>
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
  IonRefresher,
  IonRefresherContent,
  IonCardHeader,
  IonCardSubtitle,
  IonCardTitle,
  IonCardContent,
  IonListHeader
} from '@ionic/vue'
import { ref } from 'vue'
import axios from 'axios'

const randomCocktail = ref({})
const loading = ref(false)

const fetchRandomCocktail = async (displayLoaderPage) => {
  if (displayLoaderPage) {
    loading.value = true
  }
  const res = await axios.get(
    'https://www.thecocktaildb.com/api/json/v1/1/random.php'
  )
  if (res.data) {
    randomCocktail.value = res.data?.drinks[0]
  }
  loading.value = false
}

const doRefresh = (e) => {
  fetchRandomCocktail(false)
  e.target?.complete()
}

fetchRandomCocktail(true)
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
