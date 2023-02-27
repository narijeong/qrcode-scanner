<template>
  <div>
  <v-app id="inspire">
    <v-app-bar
      app
    >
      <!-- <v-app-bar-nav-icon></v-app-bar-nav-icon> -->
      <v-icon icon="mdi-qrcode" class="pl-10"></v-icon>
      <v-toolbar-title style="font-weight: 600;">QR Scan</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon>
          <v-icon>mdi-magnify</v-icon>
        </v-btn>

        <v-btn icon>
          <v-icon @click="openTest">mdi-heart</v-icon>
        </v-btn>

        <v-btn icon>
          <v-icon>mdi-dots-vertical</v-icon>
        </v-btn>
    </v-app-bar>
    <v-main>
      <div id="reader" class="pb-0"></div>
    </v-main>
    <v-footer
      app
      class="bg-indigo-lighten-1 text-center d-flex flex-column"
    >
      <div class="pt-0 pb-2">
        <v-btn
        class="ma-2"
        color="indigo"
        icon="mdi-sync"
        @click="swichCamera"
        ></v-btn>      
      </div>
      <v-divider></v-divider>
      <div>
        {{ new Date().getFullYear() }} — <strong>Boeing</strong>
      </div>
    </v-footer>
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
          color="indigo-lighten-1"
        >
          <v-btn
            icon
            dark
            @click="dialog = false"
          >
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title>IP-00RR48580Y</v-toolbar-title>
          <v-spacer></v-spacer>
        </v-toolbar>
        <v-list
          lines="three"
          subheader
        >
          <!-- <template v-slot:prepend>
          <v-icon :icon="item.icon"></v-icon>
        </template> -->
        <!-- <v-list-item
            title="TeamA, AC PRO, AC/DC"
            subtitle="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum"
          >
            <template v-slot:prepend>
              <v-btn
                class="ma-2"
                color="indigo"
                icon="mdi-cloud-upload"
                >FD1
                </v-btn>
            </template>
          </v-list-item> -->
          {{ decodedText }}
          <v-list-item title="FD 1" subtitle="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.">
          </v-list-item>
          <v-list-item title="FD 2" subtitle="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.">
          </v-list-item>
          <v-list-item title="FD 3" subtitle="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.">
          </v-list-item>
          <v-list-item title="FD 4" subtitle="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.">
          </v-list-item>
          <v-list-item title="FD 5" subtitle="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.">
          </v-list-item>
          <v-list-item title="FD 6" subtitle="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.">
          </v-list-item>
          <v-list-item title="FD 7" subtitle="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.">
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
import { stringify } from "postcss";

export default {
  data() {
        return {
            // value: "https://example.com",
            // size: 300,
            deviceIndex: 1,
            dialog: false,
            decodedText: '',
        };
  },
  methods: {
    openTest() {
      this.dialog = 1
    },
    swichCamera() {
      this.deviceIndex = this.deviceIndex == 0 ? 1 : 0
      console.log(this.deviceIndex)
    }
  },
  mounted() {
    Html5Qrcode.getCameras().then(devices => {
    var cameraId = ''
    if (devices && devices.length) {
      if (devices.length > 1) {
        cameraId = devices[this.deviceIndex].id
      }
      else {
        cameraId = devices[0].id
      }
      console.log('second', cameraId)
      // var cameraId = devices[0].id
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
          this.decodedText = decodedText
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
    // const html5QrCode = new Html5Qrcode("reader");
    // const qrCodeSuccessCallback = (decodedText, decodedResult) => {
        
    // };
    // const config = { fps: 10, qrbox: { width: 250, height: 250 } };

    // // If you want to prefer back camera
    // html5QrCode.start({ facingMode: "environment" }, config, qrCodeSuccessCallback);
    }
}
</script>

<style>
.dialog-bottom-transition-enter-active,
.dialog-bottom-transition-leave-active {
  transition: transform .2s ease-in-out;
}
</style>