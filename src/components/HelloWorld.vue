<template>
  <div>
    <div>screen capture</div>
    <button @click="userMedia">getUserMedia</button>
    <button @click="displayMedia">getDisplayMedia</button>
  </div>
</template>

<script>
import Vue from "vue";
export default Vue.extend({
  props: {
    msg: String
  },
  data: function () {
    return {
      constraints: {
        video: true,
        audio: true
      }
    }
  },
  methods: {
    userMedia: async function() {
      return await navigator.mediaDevices.getUserMedia(this.constraints).catch(err => console.log(err));
    },
    displayMedia: async function() {
      return await navigator.mediaDevices.getDisplayMedia(this.constraints).catch(err => console.log(err))
    }
  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
