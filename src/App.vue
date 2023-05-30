<template>
  <div id="app">
    <div class="cropper-container">
      <cropper-canvas background>
        <cropper-image :src="imgSrc" alt="Picture"></cropper-image>
        <cropper-shade hidden></cropper-shade>
        <cropper-handle action="move" plain></cropper-handle>
        <cropper-selection initial-coverage="0.5" aspect-ratio="1.7" ref="selection" movable resizable
          id="cropperSelection" precise="true">
          <cropper-crosshair centered></cropper-crosshair>
          <cropper-handle action="move" theme-color="rgba(255, 255, 255, 0.1)"></cropper-handle>
          <cropper-handle action="ne-resize"></cropper-handle>
          <cropper-handle action="nw-resize"></cropper-handle>
          <cropper-handle action="se-resize"></cropper-handle>
          <cropper-handle action="sw-resize"></cropper-handle>
        </cropper-selection>
      </cropper-canvas>
    </div>
    <!-- <div class="cropper-viewers">
      <cropper-viewer selection="#cropperSelection" style="width: 200px;"></cropper-viewer>
    </div> -->
    <div ref="preview" class="preview"></div>
    <input type="file" ref="imageInput" accept=".jpg,.jpeg,.png" @change="fileChanged" />
    <button @click="setImage">Set photo</button>
  </div>
</template>

<script>
// import Cropper from 'cropperjs';
import 'cropperjs';

const fileReader = new FileReader();
const image = new Image();


export default {
  name: 'App',
  data() {
    return {
      imgSrc: './assets/photo_2023-05-29_20-56-26.jpg',
      selectedFile: null,
      cropper: null,
      canvas: null,
    }
  },
  mounted() {
    image.alt = 'image';

    fileReader.onload = (event) => {
      this.imgSrc = event.target.result;
      image.src = this.imgSrc;
      // if (this.canvas) this.canvas.remove();
      // this.cropper = new Cropper(image, {
      //   container: '.cropper-container',
      // });
      // this.canvas = this.cropper.getCropperCanvas();

    };

  },
  methods: {
    fileChanged(e) {
      const files = e.target.files || e.dataTransfer.files;
      if (files.length) {
        this.selectedFile = files[0];
      }
      fileReader.readAsDataURL(this.selectedFile);
    },
    async setImage() {
      const selection = this.$refs.selection;
      // console.log('selection', selection);
      const canvas = await selection.$toCanvas();
      console.log('canvas', canvas);
      this.$refs.preview.innerHTML = "";
      this.$refs.preview.appendChild(canvas);
      // window.open(link, '_blank');
    },
    changeSelection(e) {
      console.log(e);
    },
  },

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.cropper-container {
  width: 400px;
  height: 400px;
  /* box-sizing: border-box;
  border: 1px solid #2c3e50;
  border-radius: 0.375rem;
  margin-bottom: 1rem;
  margin-top: 1rem;
  padding: 1.25rem 1.5rem; */
}

.preview {
  min-width: 200px;
  min-height: 117px;
  display: flex;
  border: 1px solid #2c3e50;
  background-color: #2c3e50;
}

.cropper-container cropper-canvas {
  width: 100%;
  height: 100%;
}
</style>
