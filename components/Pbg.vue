<template>
  <div :style="styleContent" class="pbg-Container">
    <div :style="contentStyle" class="pbg-Content">
      <slot/>
    </div>
    <div class="pbg-Mask"/>
    <transition name="fade">
      <div v-if="!loaded" :style="styleThumbnail" class="pbg-Thumbnail"/>
    </transition>
    <img v-if="!loaded" :src="src" class="pbg-TestImg" @load="onImgLoad">
  </div>
</template>

<script>
export default {
  props: {
    src: {
      type: String,
      required: true
    },
    thumbnail: {
      type: String,
      required: true
    },
    contentStyle: {
      type: String,
      default: ''
    },
    bgStyle: {
      type: String,
      default: '50% 50%/cover'
    }
  },
  data() {
    return {
      loaded: false
    }
  },
  computed: {
    styleContent() {
      return [
        { background: this.bgStyle },
        { backgroundImage: `url(${this.src})` }
      ]
    },
    styleThumbnail() {
      return [
        { background: this.bgStyle },
        { backgroundImage: `url(${this.thumbnail})` }
      ]
    }
  },
  methods: {
    onImgLoad() {
      this.loaded = true
    }
  }
}
</script>

<style scoped lang="less">
.pbg-Container {
  position: relative;
}

.pbg-Content {
  position: relative;
  z-index: 20;
}

.pbg-Mask {
  position: absolute;
  z-index: 10;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: 0;
  background-color: rgba(0, 0, 0, 0.3);
  pointer-events: none;
}

.pbg-Thumbnail {
  position: absolute;
  z-index: 1;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: 0;
}

.pbg-TestImg {
  opacity: 0;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
