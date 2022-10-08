<template>
  <div class="overlay">
      <video src="@/assets/video.webm" id="picture-in-picture" @click="requestPause"></video>
  </div>
</template>

<script>
export default {
  name: 'PictureInPicture',
  props: ['videoTime'],
  mounted(){
    let el = document.getElementById("picture-in-picture");
    el.play();
    el.currentTime = this.videoTime;
  },
  methods: {
    requestPause(){
      this.$emit("request-pause");
    }
  }
}
</script>

<style>
  .overlay{
    position: fixed;
    right: 20px;
    bottom: 20px;
  }

  .overlay video{
    width: 15rem;
    max-width: 70vw;
  }
</style>