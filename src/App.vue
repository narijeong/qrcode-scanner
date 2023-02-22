<template>
  <div>
  <v-app id="inspire">
    <v-app-bar
      app
      color="primary"
    >
      <!-- <v-app-bar-nav-icon></v-app-bar-nav-icon> -->

      <v-toolbar-title>QR Code</v-toolbar-title>

      <v-spacer></v-spacer>

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
      <!-- <template v-slot:activator="{ props }">
        <v-btn
          color="primary"
          dark
          v-bind="props"
        >
          Open Dialog
        </v-btn>
      </template> -->
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
          <v-toolbar-title>Settings</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
            <v-btn
              variant="text"
              @click="dialog = false"
            >
              Save
            </v-btn>
          </v-toolbar-items>
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
            url: String,
            ips: [{id:1, desc: 'ip 1 description'}, {id:1, desc: 'ip 1 description'}]
        };
    },
  mounted() {
    // function onScanSuccess(decodedText, decodedResult) {
    // // handle the scanned code as you like, for example:
    //   console.log(`Code matched = ${decodedText}`, decodedResult);
    // }

    // function onScanFailure(error) {
    //   // handle scan failure, usually better to ignore and keep scanning.
    //   // for example:
    //   console.warn(`Code scan error = ${error}`);
    // }

    // let html5QrcodeScanner = new Html5QrcodeScanner(
    //   "reader",
    //   { fps: 10, qrbox: {width: 250, height: 250} },
    //   /* verbose= */ false);
    // html5QrcodeScanner.render(onScanSuccess, onScanFailure);
    //   }
    //
  Html5Qrcode.getCameras().then(devices => {
  /**
   * devices would be an array of objects of type:
   * { id: "id", label: "label" }
   */
  if (devices && devices.length) {
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
        this.url = decodedText
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