<template>
  <div>
  <v-app id="inspire">
    <v-app-bar
      app
      color="primary"
    >
      <!-- <v-app-bar-nav-icon></v-app-bar-nav-icon> -->
      <div class="d-flex flex-row justify-content-center">
      <span><v-icon icon="mdi-qrcode"></v-icon></span>
      <v-toolbar-title>QR Scan</v-toolbar-title>
      </div>
      <!-- <v-spacer></v-spacer> -->

      <!-- <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn> -->
    </v-app-bar>

    <v-main>
      <div id="reader"></div>
    </v-main>
  </v-app>
  <v-row justify="center">
  <v-dialog
      v-model="dialog"
      fullscreen
      :scrim="false"
      transition="dialog-bottom-transition"
    >
      <v-card>
        <v-toolbar
          dark
          color="primary"
        >
          <v-btn
            icon
            dark
            @click="dialog = false"
          >
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title>Installation Plan  {{ this.ip }}</v-toolbar-title>
          <v-spacer></v-spacer>
        </v-toolbar>
        <v-list
          lines="two"
          subheader
        >
          <v-list-subheader>User Controls</v-list-subheader>
          <v-list-item title="Content filtering" subtitle="Set the content filtering level to restrict apps that can be downloaded"></v-list-item>
          <v-list-item title="Password" subtitle="Require password for purchase or use password to restrict purchase"></v-list-item>
        </v-list>
        <v-divider></v-divider>
        <v-list
          lines="two"
          subheader
        >
          <v-list-subheader>General</v-list-subheader>
          <v-list-item title="Notifications" subtitle="Notify me about updates to apps or games that I downloaded">
            <template v-slot:prepend>
              <v-checkbox v-model="notifications"></v-checkbox>
            </template>
          </v-list-item>
          <v-list-item title="Sound" subtitle="Auto-update apps at any time. Data charges may apply">
            <template v-slot:prepend>
              <v-checkbox v-model="sound"></v-checkbox>
            </template>
          </v-list-item>
          <v-list-item title="Auto-add widgets" subtitle="Automatically add home screen widgets">
            <template v-slot:prepend>
              <v-checkbox v-model="widgets"></v-checkbox>
            </template>
          </v-list-item>
        </v-list>
      </v-card>
    </v-dialog>
  </v-row>
  </div>
</template>


<script>
// To use Html5QrcodeScanner (more info below)
import {Html5QrcodeScanner} from "html5-qrcode"

// To use Html5Qrcode (more info below)
import {Html5Qrcode} from "html5-qrcode"

export default {
  data() {
        return {
            // value: "https://example.com",
            // size: 300,
            dialog: false,
            notifications: false,
            sound: true,
            widgets: false,
            ip: Number,
            ips: [{id:1, desc: 'ip 1 description'}, {id:1, desc: 'ip 1 description'}]
        };
    },
  mounted() {
    Html5Qrcode.getCameras().then(devices => {
    /**
     * devices would be an array of objects of type:
     * { id: "id", label: "label" }
     */
    if (devices && devices.length) {
      if (devices.length > 1) {
        var cameraId = devices[1].id;
      }

      var cameraId = devices[0].id;
      console.log(devices[0].id)
      // .. use this to start scanning.
      const html5QrCode = new Html5Qrcode(/* element id */ "reader");
      html5QrCode.start(
        cameraId, 
        {
          fps: 10,    // Optional, frame per seconds for qr code scanning
          qrbox: { width: 320, height: 320 }  // Optional, if you want bounded box UI
        },
        (decodedText, decodedResult) => {
          console.log(decodedText)
          console.log(decodedResult)
          this.ip = 1
          this.dialog = true
          // html5QrCode.stop().then((ignore) => {
          //   // QR Code scanning is stopped.
          //   }).catch((err) => {
          //     // Stop failed, handle it.
          //   });
        },
        (errorMessage) => {
          // parse error, ignore it.
        })
      .catch((err) => {
        // Start failed, handle it.
      });
    }
    }).catch(err => {
      // handle err
    });

  }
}
</script>

<style>
.dialog-bottom-transition-enter-active,
.dialog-bottom-transition-leave-active {
  transition: transform .2s ease-in-out;
}
</style>