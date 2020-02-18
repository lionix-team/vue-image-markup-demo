<template>
  <div id="app" class="main">
    <div class="editor-container">
      <div class="editor">

        <div class="tool current-color" :style="{backgroundColor: color}"></div>
        
        <div class="tool undo" @click="undo">
          <i class="fas fa-undo-alt fa-lg"></i>
        </div>
        <div class="tool redo" @click="redo">
          <i class="fas fa-redo-alt fa-lg"></i>
        </div>
        <div class="tool clear" @click="clear">
        <i class="fas fa-trash-alt fa-lg" ></i>
        </div>
        <div class="tool free-drawing" @click="setTool('freeDrawing')" :class="{'active-tool':currentActiveMethod === 'freeDrawing'}">
         <i class="fas fa-pencil-alt fa-lg"></i>
        </div>
        <div class="tool add-text" @click="setTool('text')" :class="{'active-tool':currentActiveMethod === 'text'}">
          <i class="fas fa-font fa-lg"></i>
        </div>
        <div class="tool custom-circle" @click="setTool('circle')" :class="{'active-tool':currentActiveMethod === 'circle'}">
         <i class="far fa-circle fa-lg"></i>
        </div>
        <div class="tool custom-rect" @click="setTool('rect')" :class="{'active-tool':currentActiveMethod === 'rect'}">
          <i class="far fa-square fa-lg"></i>
        </div>

        <div class="tool arrow" @click="setTool('arrow')" :class="{'active-tool':currentActiveMethod === 'arrow'}">
          <i class="fas fa-long-arrow-alt-down fa-lg"></i>
        </div>

        <div class="tool drag" @click="setTool('selectMode')" :class="{'active-tool':currentActiveMethod === 'selectMode'}">
         <i class="fas fa-arrows-alt fa-lg"></i>
        </div>
 
        <div class="tool crop-apply" v-show="croppedImage" @click="applyCropping()" :class="{'active-tool':currentActiveMethod === 'crop'}">
          <i class="far fa-check-circle fa-lg" ></i>
        </div>      
        <div class="tool crop"  v-show="!croppedImage"  @click="cropImage()">
           <i class="fas fa-crop-alt fa-lg"></i>
        </div>
         
      
         <div class="tool upload-image">
          <label for="chooseImage"><i class="fas fa-file-upload fa-lg"></i></label>
          <input id="chooseImage" style="visibility:hidden;" type="file" @change="uploadImage" accept="image/*">
        </div>
        
        <div class="tool save-image" @click="saveImage">
         <i class="fas fa-save fa-lg"></i>
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
  import Editor from 'vue-image-markup';
  import '@fortawesome/fontawesome-free/css/all.css'
  import '@fortawesome/fontawesome-free/js/all.js'
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
        imageUrl: null,
        croppedImage: false
      }
    },
    props:{
      canvasWidth:{
        default: 600
      },
      canvasHeight:{
        default: 600  
      }
      
    },
    
    mounted(){   
      if(this.imageUrl){
        this.$refs.editor.setBackgroundImage(this.imageUrl)
        this.croppedImage = this.$refs.editor.croppedImage;
      }
       this.$watch(
       () => {
            return this.$refs.editor.croppedImage
        },
      (val) => {
        this.croppedImage = val;
      }    
    )
    },
    methods:{
      cropImage(){
        this.currentActiveMethod = "crop";
        this.setTool('crop');
      },
      applyCropping(){
         this.currentActiveMethod = "";
         this.$refs.editor.applyCropping();
      },
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
        this.currentActiveMethod = this.clear;
        this.$refs.editor.clear()
      },
      undo(){
        this.currentActiveMethod = this.undo;
        this.$refs.editor.undo();
      },
      redo(){
        this.currentActiveMethod = this.redo;
        this.$refs.editor.redo();
      }

    }
  }
</script>

<style>
  @import "./app.css";
</style>
