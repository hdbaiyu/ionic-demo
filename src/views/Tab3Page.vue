<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>推送通知</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content fullscreen>
      <div class="ion-padding">
        <p>应用已启用推送通知</p>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
// import ExploreContainer from '@/components/ExploreContainer.vue';
import { onMounted } from 'vue';
import { PushNotifications } from '@capacitor/push-notifications';

const registerPush = async () => {
  await PushNotifications.requestPermissions();
  await PushNotifications.register();

  PushNotifications.addListener('registration', (token) => {
    console.log('注册成功:', token.value);
  });

  PushNotifications.addListener('registrationError', (error) => {
    console.error('注册失败:', error);
  });

  PushNotifications.addListener('pushNotificationReceived', (notification) => {
    console.log('收到推送:', notification);
    alert(`通知: ${notification.title} - ${notification.body}`);
  });
};

onMounted(() => {
  registerPush();
});

</script>
<style scoped>
.ion-padding {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
