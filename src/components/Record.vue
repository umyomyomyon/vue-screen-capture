<template>
  <div>
    <div>screen capture</div>
    <div class="video-container">
      <video :srcObject.prop="displayStream" autoplay></video>
      <video class="inner-video" :srcObject.prop="userStream" autoplay></video>
    </div>
    <button @click="startCapture">CAPTURE!</button>
    <button @click="stopCapture">STOP!</button>
  </div>
</template>

<script>
import Vue from "vue";

export default Vue.extend({
  data: function () {
    return {
      constraints: {
        video: {
          cursor: "always"
        },
        audio: false
      },
      displayStream: undefined,
      userStream: undefined,
      capturing: false
    }
  },
  methods: {
    startCapture: async function () {
      this.userStream = await navigator.mediaDevices.getUserMedia({video: true, audio: true})
      this.displayStream = await navigator.mediaDevices.getDisplayMedia(this.constraints).catch(err => console.log(err))
      this.capturing = true
    },
    stopCapture: function () {
      const tracks = this.makeTracks()
      tracks.forEach(track => track.stop())
      this.cleanUpStreams()
      this.capturing = false
    },
    makeTracks: function () {
      const displayStreamTracks = this.displayStream.getTracks()
      const userStreamTracks = this.userStream.getTracks()
      return [...displayStreamTracks, ...userStreamTracks]
    },
    cleanUpStreams: function () {
      this.displayStream = undefined;
      this.userStream = undefined;
    }
  }
});
</script>

<style scoped>
.video-container {
  position: relative;
}

video {
  height: 540px;
  width: 960px;
}

.inner-video {
  height: 200px;
  width: 200px;
  position: absolute;
  right: 400px;
}
</style>
