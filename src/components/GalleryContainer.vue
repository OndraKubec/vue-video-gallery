<template>
  <div class="gallery-container">
    <video
      loop
      class="video-item"
      src="@/assets/video.webm"
      v-for="index in 48"
      :key="index"
      :index="index"
      @click="playPause($event)"
    ></video>
  </div>
</template>

<script>
export default {
  name: "GalleryContainer",
  props: ["requestPauseTrigger"],
  data() {
    return {
      videoIsPlaying: false,
      videoPlayingIndex: null,
      pictureInPictureIsEnabled: false,
    };
  },
  methods: {
    playPause(e) {
      let video = e.target;
      let allVideos = document.getElementsByTagName("video");

      if (video.paused) {
        for (var i = 0; i < allVideos.length; i++) {
          allVideos[i].pause();
        }
        this.disablePictureInPicture();
        video.play();
        this.videoIsPlaying = true;
        this.videoPlayingIndex = video.getAttribute("index");
      } else {
        video.pause();
        this.videoIsPlaying = false;
      }
    },
    isInVieport() {
      let currentVideoEl = document.querySelector(
        "video[index='" + this.videoPlayingIndex + "']"
      );
      let viewportOffset = currentVideoEl.getBoundingClientRect();
      let top = viewportOffset.top;
      if (top + currentVideoEl.offsetHeight < 1 || top > window.innerHeight)
        return false;
      return true;
    },
    enablePictureInPicture() {
      let currentVideoEl = document.querySelector(
        "video[index='" + this.videoPlayingIndex + "']"
      );
      this.$emit("picture-in-picture", [true, currentVideoEl.currentTime]);
    },
    disablePictureInPicture() {
      this.$emit("picture-in-picture", [false, 0]);
    },
  },
  mounted() {
    window.onscroll = () => {
      if (this.videoIsPlaying == true && this.isInVieport() == false) {
        // running video is out of viewport
        if (this.pictureInPictureIsEnabled != true)
          this.enablePictureInPicture();
        this.pictureInPictureIsEnabled = true;
      } else {
        if (this.pictureInPictureIsEnabled != false)
          this.disablePictureInPicture();
        this.pictureInPictureIsEnabled = false;
      }
    };
  },
  watch: {
    requestPauseTrigger() {
      let allVideos = document.getElementsByTagName("video");
      for (var i = 0; i < allVideos.length; i++) {
        allVideos[i].pause();
      }
      this.videoIsPlaying = false;
      this.disablePictureInPicture();
    },
  },
};
</script>

<style>
.gallery-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 15px;
  padding: 15px;
  background-color: #4378b5;
}
.gallery-container .video-item {
  width: 100%;
}
</style>