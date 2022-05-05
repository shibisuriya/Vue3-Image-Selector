<template>
  <div class="image-selector-container">
    <div v-if="showFileSelector">
      <div class="drag-and-drop">Drag & Drop</div>
      <input
        type="text"
        class="url"
        v-model="url"
        v-if="fileType == 'url'"
        placeholder="URL"
      />

      <div class="selectors-container">
        <div>
          <input
            v-model="fileType"
            type="radio"
            id="file"
            name="inputType"
            value="file"
          />
          <label for="file">File</label>
        </div>
        <div>
          <input
            v-model="fileType"
            type="radio"
            id="webcam"
            name="inputType"
            value="webcam"
          />
          <label for="webcam">Webcam</label>
        </div>
        <div>
          <input
            v-model="fileType"
            type="radio"
            id="url"
            name="inputType"
            value="url"
          />
          <label for="url">URL</label>
        </div>
        <div>
          <input
            v-model="fileType"
            type="radio"
            id="pdf"
            name="inputType"
            value="pdf"
          />
          <label for="pdf">PDF</label>
        </div>
      </div>
      <button class="proceed-button" @click="optionSelected">Proceed</button>
      <input
        type="file"
        @input="fileChanged"
        :hidden="true"
        accept="image/*"
        ref="file"
      />
      <input
        type="file"
        @input="pdfChanged"
        :hidden="true"
        accept=".pdf"
        ref="pdf"
      />
    </div>
    <ImageCropper
      @cancel="reset"
      v-if="showCropper"
      :base64image="base64image"
      @recieveCroppedImage="recieveCroppedImage"
    ></ImageCropper>
    <ImageComponent
      v-if="showCroppedImage"
      :image="croppedImage"
      @reset="reset"
    ></ImageComponent>
    <WebcamComponent
      v-if="showWebcam"
      @imageCaptured="recieveWebcamImage"
    ></WebcamComponent>
  </div>
</template>

<script>
import ImageCropper from "./ImageCropper.vue";
import ImageComponent from "./ImageComponent.vue";
import WebcamComponent from "./WebcamComponent.vue";
export default {
  data() {
    return {
      fileType: "file",
      url: "",
      showCropper: false,
      base64image: "",
      showFileSelector: true,
      croppedImage: "",
      showCroppedImage: false,
      showWebcam: false,
    };
  },
  components: {
    ImageCropper,
    ImageComponent,
    WebcamComponent,
  },
  methods: {
    reset() {
      this.fileType = "file";
      this.url = "";
      this.showCropper = false;
      this.base64image = "";
      this.showFileSelector = true;
      this.croppedImage = "";
      this.showCroppedImage = "";
    },
    recieveWebcamImage(base64image) {
      this.base64image = base64image;
      this.showCropper = true;
      this.showWebcam = false;
      this.showFileSelector = false;
    },
    recieveCroppedImage(croppedImage) {
      this.croppedImage = croppedImage;
      this.showCropper = false;
      this.showFileSelector = false;
      this.showCroppedImage = true;
    },
    optionSelected() {
      this.url = "";
      switch (this.fileType) {
        case "file":
          this.$refs.file.click();
          break;
        case "pdf":
          this.$refs.pdf.click();
          break;
        case "webcam":
          this.showWebcam = true;
          break;
        case "url":
          this.processUrl();
          break;
      }
    },
    file() {
      console.log("file");
    },
    pdf() {
      console.log("pdf");
    },
    processUrl() {
      console.log("url");
    },
    fileChanged(e) {
      const selectedImage = e.target.files[0]; // Only select the first file...
      const reader = new FileReader();
      reader.onload = (e) => {
        this.base64image = e.target.result;
        this.showCropper = true;
        this.showFileSelector = false;
      };
      reader.readAsDataURL(selectedImage);
    },
    pdfChanged(e) {
      console.log(e);
    },
  },
};
</script>

<style scoped>
.selectors-container {
  display: flex;
  justify-content: space-evenly;
  max-width: 300px;
  border: 1px solid black;
  padding: 1em;
}
.drag-and-drop {
  border: 1px solid black;
  text-align: center;
  padding: 1em;
  margin-bottom: 1em;
}
.image-selector-container {
  width: 300px;
  border: 1px solid black;
  padding: 1em;
}
.proceed-button {
  display: block;
  margin: 0 auto;
  margin-top: 1em;
  padding: 0.3em;
}
.url {
  margin: 0 auto;
  display: block;
  width: 100%;
  box-sizing: border-box;
  margin-bottom: 1em;
  padding: 1em;
  text-align: center;
}
</style>