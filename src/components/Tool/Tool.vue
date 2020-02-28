<template>
  <div class="tool" >
    <div @click="event" v-if="!labelForUploadImage">
      <i :class="iconClass"></i>
    </div>
    <div v-if="labelForUploadImage" :class="{ 'upload-image': labelForUploadImage,'tool': labelForUploadImage}">
      <label for="chooseImage"><i :class="iconClass"></i></label>
      <input
        id="chooseImage"
        style="visibility:hidden;"
        type="file"
        @change="event"
        accept="image/*"
      />
    </div>
  </div>
</template>

<script>    
export default {
  name: "tool",
  props: ["event", "iconClass", "labelForUploadImage"],
};
</script>

<style scoped lang="scss">
@import "./tool.scss";
</style>
