<template>
  <div class="image-selector-container">
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
    <ImageCropper v-if="showCropper" :base64image="base64image"></ImageCropper>
  </div>
</template>

<script>
import ImageCropper from "./ImageCropper.vue";
export default {
  data() {
    return {
      fileType: "file",
      url: "",
      showCropper: false,
      base64image: "",
    };
  },
  components: {
    ImageCropper,
  },
  methods: {
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
          this.webcam();
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
    webcam() {
      console.log("webcam");
    },
    processUrl() {
      console.log("url");
    },
    fileChanged(e) {
      const selectedImage = e.target.files[0]; // Only select the first file...
      const reader = new FileReader();
      reader.onload = (e) => {
        this.base64image = e.target.result;
        console.log(this.base64image);
        this.showCropper = true;
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