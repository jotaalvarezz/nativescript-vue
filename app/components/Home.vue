<template>
    <Page>
        <ActionBar backgroundColor="#3c495e">
            <Label text="Jikan Anime" class="cardtitle" fontSize="18" />
            <ActionItem android.systemIcon="ic_menu_camera" ios.position="left" text="scan code" @tap="readCode" />
            <ActionItem android.systemIcon="ic_menu_search" ios.position="right" text="delete" @tap="modalSearch" />
        </ActionBar>
        <TabView :selectedIndex="selectedIndex" @selectedIndexChange="indexChange" androidTabsPosition="buttom">
            <TabViewItem title="Tab 1">
                <Animes></Animes>
            </TabViewItem>
            <TabViewItem title="Tab 2">
                <!-- <Scan></Scan> -->
                <Label text="Content for Tab 2" />
                <!-- <GridLayout columns="*" rows="auto, auto, auto, auto">
                    <Label row="0" class="message" text="Check the console log for scanned barcodes"
                        textWrap="true"></Label>

                    <BarcodeScanner row="1" height="300" formats="QR_CODE, EAN_13, UPC_A" beepOnScan="true"
                        reportDuplicates="true" preferFrontCamera="false" @scanResult="onScanResult" v-if="isIOS">
                    </BarcodeScanner>

                    <Button row="2" class="btn btn-primary btn-rounded-sm" text="back camera, with flip"
                        @tap="doScanWithBackCameraWithFlip"></Button>
                    <Button row="3" class="btn btn-primary btn-rounded-sm" text="front camera, no flip"
                        @tap="doScanWithFrontCameraNoFlip"></Button>

                </GridLayout> -->
            </TabViewItem>
        </TabView>
        <!-- <Animes></Animes> -->
    </Page>
</template>

<script>
import { mapState } from 'vuex';
import { mapMutations } from 'vuex';
import axios from 'axios';
import AnimeInfo from './AnimeInfo.vue';
import Animes from './Animes.vue'
import Scan from './scan_code/ScanCode.vue'
import { isIOS } from "@nativescript/core";
import { BarcodeScanner } from "nativescript-barcodescanner";

export default {
    components: {
        Animes,
        Scan
    },

    data() {
        return {
            tab_item: 0, activate: false,
            isIOS
        }
    },
    computed: {
    
    },

    methods: {
        onScanResult(evt) {
            console.log(`onScanResult: ${evt.text} (${evt.format})`);
        },
        doScanWithBackCameraWithFlip() {
            this.scan(false, true);
        },
        doScanWithFrontCameraNoFlip() {
            this.scan(true, false);
        },
        scan(preferFrontCamera, showFlipCameraButton) {
            new BarcodeScanner().scan({
                cancelLabel: "EXIT. Also, try the volume buttons!", // iOS only, default 'Close'
                cancelLabelBackgroundColor: "#333333", // iOS only, default '#000000' (black)
                message: "Use the volume buttons for extra light", // Android only, default is 'Place a barcode inside the viewfinder rectangle to scan it.'
                preferFrontCamera,            // Android only, default false
                showFlipCameraButton,         // default false
                showTorchButton: true,        // iOS only, default false
                torchOn: false,               // launch with the flashlight on (default false)
                resultDisplayDuration: 500,   // Android only, default 1500 (ms), set to 0 to disable echoing the scanned text
                beepOnScan: true,             // Play or Suppress beep on scan (default true)
                openSettingsIfPermissionWasPreviouslyDenied: true, // On iOS you can send the user to the settings app if access was previously denied
                closeCallback: () => {
                    console.log("Scanner closed @ " + new Date().getTime());
                }
            }).then(
                function (result) {
                    console.log("--- scanned: " + result.text);
                    // Note that this Promise is never invoked when a 'continuousScanCallback' function is provided
                    setTimeout(function () {
                        // if this alert doesn't show up please upgrade to {N} 2.4.0+
                        alert({
                            title: "Scan result",
                            message: "Format: " + result.format + ",\nValue: " + result.text,
                            okButtonText: "OK"
                        });
                    }, 500);
                },
                function (errorMessage) {
                    console.log("No scan. " + errorMessage);
                }
            );
        },
    },

    created() {
      
    },
};
</script>

<style scoped lang="scss">
@import '@nativescript/theme/scss/variables/blue';

// Custom styles
.fas {
    @include colorize($color: accent);
}

.info {
    font-size: 40;
}

.cardtitle {
    text-align: center;
    font-weight: bold;
    color: #fff;
}

/* ion-searchbar {
  input {
    border-radius: 30px !important;
  }
} */

.search {
    margin-top: 5px;
    border-top-right-radius: 30;
}

.position {}
</style>
