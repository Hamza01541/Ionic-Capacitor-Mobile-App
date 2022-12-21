<template>
  <ion-page>
    <ion-header class="ion-no-border">
      <ion-toolbar>
        <ion-title>Login</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <div id="container">
        <ion-button @click="login">
          Login
        </ion-button>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  IonButton,
  loadingController,
} from "@ionic/vue";
import { defineComponent } from "vue";
import { Plugins } from "@capacitor/core";
const { MsAuthPlugin } = Plugins;

export default defineComponent({
  name: "LoginPage",
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButton,
  },
  methods: {
    async login() {
      const loading = await loadingController.create({
        message: "please wait",
      });
      await loading.present();
      const result = await MsAuthPlugin.login({
        clientId: "6a6f31cb-6d9c-4906-b054-5dee86a234db",
        tenant: "1c7ef0e8-1ff2-4ac9-9c20-944a0297e57e",
        scopes: [
          "https://fanfedertennisdev.onmicrosoft.com/6a6f31cb-6d9c-4906-b054-5dee86a234db/read",
        ],
        keyHash: "aznYGQwKMkkjd5PIVEPc+2TKNzk=",
        authorityUrl:
          "https://fanfedertennisdev.b2clogin.com/tfp/fanfedertennisdev.onmicrosoft.com/b2c_1_digital_platform_4_signupsignin",
        authorityType: "B2C",
      });
      const accessToken = await result.accessToken;
      console.log(accessToken);
      if (accessToken) {
        loading.dismiss();
        this.$router.push({
          path: "/home",
          query: { tokenData: JSON.stringify(accessToken) },
        });
      }
    },
  },
});
</script>

<style scoped>
#container {
  text-align: center;

  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;

  color: #8c8c8c;

  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
