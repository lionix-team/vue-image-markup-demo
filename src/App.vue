<template>
  <div id="app" class="main">
    <div class="editor-container">
      <div class="editor">

        <div class="tool current-color" :style="{backgroundColor: color}"></div>
        <div class="tool free-drawing" @click="setTool('freeDrawing')" :class="{'active-tool':currentActiveMethod === 'freeDrawing'}">
          <img src="./assets/brush.png" alt="brush">
        </div>
        <div class="tool add-text" @click="setTool('text')" :class="{'active-tool':currentActiveMethod === 'text'}">
          <img src="./assets/text.png" alt="text">
        </div>
        <div class="tool custom-circle" @click="setTool('circle')" :class="{'active-tool':currentActiveMethod === 'circle'}">
          <img src="./assets/circle.png" alt="circle">
        </div>
        <div class="tool custom-rect" @click="setTool('rect')" :class="{'active-tool':currentActiveMethod === 'rect'}">
          <img src="./assets/rectangle.png" alt="rect">
        </div>

        <div class="tool arrow" @click="setTool('arrow')" :class="{'active-tool':currentActiveMethod === 'arrow'}">
          <img src="./assets/arrow.png" alt="arrow">
        </div>

        <div class="tool drag" @click="setTool('selectMode')" :class="{'active-tool':currentActiveMethod === 'selectMode'}">
          <img src="./assets/drag.png" alt="drag">
        </div>
        <div class="tool undo" @click="undo">
          <img src="./assets/undo.png" alt="undo">
        </div>
        <div class="tool redo" @click="redo">
          <img src="./assets/redo.png" alt="redo">
        </div>
        <div class="tool upload-image">
          <label for="chooseImage"><img src="./assets/download.png" alt="download"></label>
          <input id="chooseImage" style="visibility:hidden;" type="file" @change="uploadImage" accept="image/*">
        </div>
        <div class="tool save-image" @click="saveImage">
          <img src="./assets/save.png" alt="save" width="24" height="24">
        </div>
        <div class="tool clear" @click="clear">
          <img src="./assets/clear.png" alt="clear">
        </div>
      </div>
      <Editor :canvasWidth="canvasWidth" :canvasHeight="canvasHeight" ref="editor"/>
    </div>
    <div class="colors">
      <div class="color-container red" @click="changeColor('#e40000')"></div>
      <div class="color-container yellow" @click="changeColor('#e8eb34')"></div>
      <div class="color-container purple" @click="changeColor('#a834eb')"></div>
      <div class="color-container green" @click="changeColor('#65c31a')"></div>
      <div class="color-container lightblue" @click="changeColor('#34b7eb')"></div>
      <div class="color-container pink" @click="changeColor('#eb34df')"></div>
      <div class="color-container blue" @click="changeColor('#1a10ad')"></div>
      <div class="color-container black" @click="changeColor('#000000')"></div>
    </div>
  </div>
</template>

<script>
  import Editor from 'vue-image-markup'
  export default {
    name: 'app',
    components: {
      Editor
    },
    data(){
      return{
        currentActiveMethod: null,
        params: {},
        color: "black",
        imageUrl: null
      }
    },
    props:{
      canvasWidth:{
        default: 600
      },
      canvasHeight:{
        default: 420
      }

    },
    mounted(){
      if(this.imageUrl){
        this.$refs.editor.setBackgroundImage(this.imageUrl)
      }
    },
    methods:{
      changeColor(colorHex){
        this.color = colorHex;
        this.$refs.editor.changeColor(colorHex);
      },
      saveImage(){
        let image = this.$refs.editor.saveImage();
        this.saveImageAsFile(image)
      },
      saveImageAsFile(base64) {
        let link = document.createElement("a");
        link.setAttribute("href", base64);
        link.setAttribute("download", "image-markup");
        link.click();
      },
      setTool(type,params){
        this.currentActiveMethod = type;
        this.$refs.editor.set(type,params)
      },
      uploadImage(e){
        this.$refs.editor.uploadImage(e)
      },
      clear(){
        this.$refs.editor.clear()
      },
      undo(){
        this.$refs.editor.undo();
      },
      redo(){
        this.$refs.editor.redo();
      }

    }
  }
</script>

<style>
  @import "./app.css";
</style>
