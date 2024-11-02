<template>
  <div>
    <canvas ref="barcodeCanvas"></canvas>
  </div>
</template>

<script>
import bwipjs from "bwip-js";

export default {
  name: "BarcodeUser",
  props: {
    value: {
      type: String,
      required: true,
    },
  },
  mounted() {
    this.generateBarcode(this.value);
  },
  methods: {
    generateBarcode(value) {
      bwipjs.toCanvas(this.$refs.barcodeCanvas, {
        bcid: "code128", // Barcode type
        text: value, // Text to encode
        scale: 3, // Scaling factor
        height: 10, // Height of the barcode
        width: 1, // Width of the barcode
        inventor: {
          font: "Helvetica", // Choose your font...
        },
      });
    },
  },
};
</script>

<style scoped>
canvas {
  margin-top: 10px;
}
</style>
