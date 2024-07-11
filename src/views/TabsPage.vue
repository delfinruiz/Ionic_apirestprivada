<template>
  <ion-page>
    <ion-tabs>
      <ion-router-outlet></ion-router-outlet>
      <ion-tab-bar slot="bottom">
        <ion-tab-button tab="addstudent" href="/tabs/addstudent">
          <ion-icon aria-hidden="true" :icon="addCircleOutline" />
          <ion-label>Agregar</ion-label>
        </ion-tab-button>

        <ion-tab-button tab="liststudent" href="/tabs/liststudent">
          <ion-icon aria-hidden="true" :icon="personCircleOutline" />
          <ion-label>Listar</ion-label>
        </ion-tab-button>

      </ion-tab-bar>
    </ion-tabs>
  </ion-page>
</template>

<script setup>
import { IonTabBar, IonTabButton, IonTabs, IonLabel, IonIcon, IonPage, IonRouterOutlet } from '@ionic/vue';
import { addCircleOutline, personCircleOutline } from 'ionicons/icons';
</script>
