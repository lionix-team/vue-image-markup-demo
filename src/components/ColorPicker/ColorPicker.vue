<template>
  <div class="color-picker" :style="{ backgroundColor: color ? color : '' }" @click="event(color)"></div>
</template>
<script>
export default {
  name: "colorPicker",
  props: ["color","event"],
};
</script>

<style scoped lang="scss">
@import "./colorpicker.scss";
</style>
