<template>
  <div class="container">
    <GalleryContainer
      @picture-in-picture="isPictureInPicture"
      :requestPauseTrigger="requestPauseTrigger"
    />
    <PictureInPicture
      v-if="showPictureInPicture"
      :videoTime="videoTime"
      @request-pause="requestPause"
    />
  </div>
</template>

<script>
import GalleryContainer from "./components/GalleryContainer.vue";
import PictureInPicture from "./components/PictureInPicture.vue";

export default {
  name: "App",
  data() {
    return {
      showPictureInPicture: false,
      videoTime: 0,
      requestPauseTrigger: 0,
    };
  },
  components: {
    GalleryContainer,
    PictureInPicture,
  },
  methods: {
    isPictureInPicture(e) {
      if (e[0]) {
        this.showPictureInPicture = true;
        this.videoTime = e[1];
      } else {
        this.showPictureInPicture = false;
      }
    },
    requestPause() {
      this.requestPauseTrigger = this.requestPauseTrigger + 1;
    },
  },
};
</script>

<style>
body {
  margin: 2rem 0;
  background-color: #9ec2ec;
}
.container {
  width: 80%;
  margin: 0 auto;
}
</style>
