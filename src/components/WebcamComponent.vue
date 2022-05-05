<template>
  <div>
    <video
      v-show="stream"
      :srcObject="stream"
      autoplay
      muted
      style="width: 100%; display: block"
    ></video>
    <img :src="capturedPhoto" alt="" style="width: 100%; display: block" />
    <button @click="capturePhoto">Capture</button>
  </div>
</template>
<script>
export default {
  components: {},
  data() {
    return {
      stream: null,
      track: null,
      imageCapture: null,
      capturedPhoto: null,
    };
  },
  mounted() {
    const constraints = { audio: true, video: true };
    this.getCamera(constraints);
  },
  methods: {
    capture() {},
    async getCamera(constraints) {
      try {
        this.stream = await navigator.mediaDevices.getUserMedia(constraints);
        this.track = this.stream.getVideoTracks()[0];
        this.imageCapture = new ImageCapture(this.track);
      } catch (err) {
        this.stream = null;
        console.log(err);
      }
    },
    async capturePhoto() {
      try {
        const blob = await this.imageCapture.takePhoto();
        const reader = new FileReader();
        reader.onloadend = () => {
          const base64image = reader.result;
          this.$emit("imageCaptured", base64image);
        };
        reader.readAsDataURL(blob);
      } catch (error) {
        console.log;
      }
    },
  },
};
</script>

 
