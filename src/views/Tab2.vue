<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Photo Gallery</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-grid>
        <ion-row>
          <ion-col size="6"
                   :key="photo"
                   v-for="photo in photos">
            <!-- <ion-img :src="photo.webviewPath"></ion-img> -->
            <ion-img :src="photo.webviewPath"
                     @click="showActionSheet(photo)"></ion-img>
          </ion-col>
        </ion-row>
      </ion-grid>
      <ion-fab vertical="bottom"
               horizontal="center"
               slot="fixed">
        <ion-fab-button @click="takePhoto()">
          <ion-icon :icon="camera"></ion-icon>
        </ion-fab-button>
      </ion-fab>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
// import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import {
  actionSheetController,
  IonPage,
  IonHeader,
  IonFab,
  IonFabButton,
  IonIcon,
  IonToolbar,
  IonTitle,
  IonContent,
  IonGrid,
  IonRow,
  IonCol,
  IonImg,
} from '@ionic/vue';
import { camera, trash, close } from 'ionicons/icons';
// import { usePhotoGallery } from '@/composables/usePhotoGallery';
import { usePhotoGallery, UserPhoto } from '@/composables/usePhotoGallery';

export default defineComponent({
  name: 'Tab2Gallery',
  components: {IonHeader, IonToolbar, IonTitle, IonContent, IonPage },
  setup(){
    // const { takePhoto } = usePhotoGallery();
    // const { photos, takePhoto } = usePhotoGallery();
    const { photos, takePhoto, deletePhoto } = usePhotoGallery();

    const showActionSheet = async (photo: UserPhoto) => {
      const actionSheet = await actionSheetController.create({
        header: 'Photos',
        buttons: [
          {
            text: 'Delete',
            role: 'destructive',
            icon: trash,
            handler: () => {
              deletePhoto(photo);
            },
          },
          {
            text: 'Cancel',
            icon: close,
            role: 'cancel',
            handler: () => {
              // Nothing to do, action sheet is automatically closed
            },
          },
        ],
      });
      await actionSheet.present();
    };

    return {
      photos,
      takePhoto,
      camera,
      trash,
      close,
      showActionSheet,
    }
  },
});
</script>
