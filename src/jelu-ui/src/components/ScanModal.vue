<script setup lang="ts">
import { ref } from "vue";
import { StreamBarcodeReader } from "@teckel/vue-barcode-reader";
import { useI18n } from 'vue-i18n';

const { t } = useI18n({
      inheritLocale: true,
      useScope: 'global'
    })

const emit = defineEmits<{
  (e: 'close'): void,
  (e: 'decoded', barcode: string|null): void
}>()

const decodedText = ref("");

const acceptBarcode = () => {
    emit('decoded', decodedText.value)
    emit('close')
}

const onLoaded = () => {
  console.log("barcode modal loaded");
};
const onDecode = (text: string) => {
  console.log("barcode " + text);
  decodedText.value = text;
  acceptBarcode()
};

</script>

<template>
  <section class="edit-modal">
    <div class="grid justify-center justify-items-center">
      <div class="mb-2">
        <h1 class="text-2xl typewriter capitalize">
          {{ t('labels.import_book') }}
        </h1>
      </div>
      <div>
        <div class="field mb-2">
          <StreamBarcodeReader
            torch
            no-front-cameras
            @decode="onDecode"
            @loaded="onLoaded"
          />
          <p>{{ decodedText }}</p>
        </div>
      </div>
    </div>
  </section>
</template>

<style lang="scss">

</style>
