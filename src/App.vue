<template>
  <v-app id="inspire">
    <v-app-bar
      app
    >
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

      <v-toolbar-title>QR Code</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
        <!-- <v-row>
          <v-col
            v-for="n in 24"
            :key="n"
            cols="4"
          >
            <v-card height="200"></v-card>
          </v-col>
        </v-row> -->
        <div id="reader" class="ml-0"></div>
    </v-main>
  </v-app>
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
            url: String,
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
        html5QrCode.stop().then((ignore) => {
          // QR Code scanning is stopped.
          }).catch((err) => {
            // Stop failed, handle it.
          });
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
