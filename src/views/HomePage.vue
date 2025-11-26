<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar color="primary">
        <ion-title>Crypto Tracker</ion-title>
        <ion-buttons slot="end">
          <ion-button @click="loadCryptoData"> Refresh </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <div v-if="loading" class="loading-text">
        <p>Mengambil data terbaru...</p>
      </div>

      <ion-list v-else>
        <ion-item v-for="coin in coins" :key="coin.id" class="coin-item">
          <div class="rank-badge" slot="start">
            <small>Rank</small>
            <h2>{{ coin.rank }}</h2>
          </div>

          <ion-label>
            <h2>{{ coin.symbol }}</h2>
            <p>{{ coin.name }}</p>
          </ion-label>

          <div class="price-tag" slot="end">
            <small>USD</small>
            <h3>{{ coin.price_usd }}</h3>
          </div>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  IonList,
  IonItem,
  IonLabel,
  IonButtons,
  IonButton,
} from "@ionic/vue";
import { ref, onMounted } from "vue";

const coins = ref([]);
const loading = ref(false);

const loadCryptoData = async () => {
  loading.value = true;
  try {
    const response = await fetch("https://api.coinlore.net/api/tickers/");
    const json = await response.json();
    coins.value = json.data;
    console.log("Data loaded!", coins.value);
  } catch (error) {
    console.error("Error:", error);
    alert("Gagal mengambil data!");
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  loadCryptoData();
});
</script>

<style scoped>
.loading-text {
  text-align: center;
  margin-top: 20px;
  color: #666;
}

.rank-badge {
  text-align: center;
  min-width: 40px;
  margin-right: 10px;
}
.rank-badge small {
  font-size: 10px;
  color: #888;
  display: block;
}
.rank-badge h2 {
  font-size: 18px;
  font-weight: bold;
  margin: 0;
  color: #333;
}

ion-label h2 {
  font-weight: 800;
  color: #000;
}

.price-tag {
  text-align: right;
}
.price-tag small {
  font-size: 10px;
  color: #888;
  display: block;
}
.price-tag h3 {
  font-size: 16px;
  font-weight: bold;
  color: blue;
  margin: 0;
}

ion-item:nth-child(even) {
  --background: #f9f9f9;
}
</style>
