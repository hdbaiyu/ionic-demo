<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>相机拍照</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content fullscreen>
      <ion-button expand="full" @click="takePhoto">拍照</ion-button>
      <ion-img v-if="picture??picture?.webPath" :src="picture?.webPath" alt="Captured photo" />
      <ion-button expand="full" @click="getPosition">获取位置</ion-button>
      <ion-text v-if="localPosition">
        {{ localPosition.coords.latitude }}, {{ localPosition.coords.longitude }}
      </ion-text>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
import { cameraOutline } from 'ionicons/icons';
// import { Geolocation } from '@capacitor/geolocation';
import { Camera, CameraResultType, CameraSource } from '@capacitor/camera';
import { Geolocation } from '@capacitor/geolocation';
import { ref } from 'vue';
const localPosition = ref();
const picture = ref(null);
const takePhoto = async () => {
  const image = await Camera.getPhoto({
    quality: 90,
    allowEditing: true,
    resultType: CameraResultType.Uri
  });
  picture.value = image;
};
const getPosition = async () => {
  const position = await Geolocation.getCurrentPosition();
  localPosition.value = position;
};
// getPosition();
</script>
<style scoped>
ion-img {
  display: block;
  margin: 20px auto;
  max-width: 90%;
}
</style>
