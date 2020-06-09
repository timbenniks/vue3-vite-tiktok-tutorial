<template>
  <div class="tiktok">
    <video :src="video" ref="vidRef" @click="togglePlay" />
    <svg
      width="512"
      height="512"
      viewBox="0 0 512 512"
      @click="togglePlay"
      v-show="!state.playing"
    >
      <path d="M152.443 136.417l207.114 119.573-207.114 119.593z" fill="#fff" />
    </svg>
  </div>
</template>

<script>
import { defineComponent, ref, reactive } from "vue";

export default defineComponent({
  name: "tiktok",
  props: {
    video: String,
  },
  setup() {
    const vidRef = ref(null);

    const state = reactive({
      playing: false,
    });

    const play = () => {
      vidRef.value.play();
      state.playing = true;
    };

    const pause = () => {
      vidRef.value.pause();
      state.playing = false;
    };

    const togglePlay = () => {
      if (state.playing) {
        pause();
      } else {
        play();
      }
    };

    return {
      vidRef,
      play,
      pause,
      togglePlay,
      state,
    };
  },
});
</script>

<style scoped>
.tiktok {
  position: relative;
  width: 100%;
  height: 100%;
}

video {
  aspect-ratio: 348/520;
  width: 100%;
  height: auto;
  position: relative;
}

svg {
  position: absolute;
  top: calc(50% - 35px);
  left: calc(50% - 35px);
  width: 90px;
  height: 90px;
  z-index: 10;
}
</style>
