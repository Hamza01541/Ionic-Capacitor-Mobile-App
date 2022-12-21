<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Home</ion-title>
        <ion-buttons slot="end">
          <ion-button v-on:click="logOut()"> LOGOUT </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <div id="container">
        <ion-list>
          <ion-label>{{ tokenData }}</ion-label>
          <p>{{ token }}</p>
        </ion-list>
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
  IonButtons,
  IonButton,
} from "@ionic/vue";
import { defineComponent } from "vue";
import jwt_decode from "jwt-decode";
import { Plugins } from "@capacitor/core";
const { MsAuthPlugin } = Plugins;

export default defineComponent({
  name: "HomePage",
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButtons,
    IonButton,
  },
  data() {
    return { tokenData: new Object(), token: "" };
  },
  methods: {
    async logOut() {
      const result = await MsAuthPlugin.logout({
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
      this.$router.back();
    },
  },
  created() {
    const temp: any = this.$router.currentRoute.value.query;
    this.$data.token = JSON.parse(temp.tokenData);
    this.$data.tokenData = jwt_decode(temp.tokenData);
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
  font-size: 12px;
  line-height: unset;
  color: #8c8c8c;

  margin: 0;
}

#container a {
  text-decoration: none;
}
ion-label {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
