<template>
  <div>
    <img class="image" :src="image" alt="Cropped Image" />
    <button @click="reset">Reset</button>
  </div>
</template>

<script>
export default {
  mounted() {},
  props: ["image"],
  methods: {
    reset() {
      this.$emit("reset");
    },
  },
};
</script>

<style  scoped>
.image {
  width: 100%;
}
</style>