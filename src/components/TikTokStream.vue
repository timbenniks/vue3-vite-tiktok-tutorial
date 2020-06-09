<template>
  <div
    class="tiktok-stream"
    ref="tiktokStream"
    v-swipe="onSwipe"
    :style="state.style"
  >
    <tiktok
      v-for="(tiktok, i) in tiktokData"
      :video="tiktok.video"
      :key="tiktok.id"
      :ref="
        (el) => {
          tiktokRefs[i] = el;
        }
      "
    />
  </div>
</template>

<script>
import {
  defineComponent,
  ref,
  reactive,
  computed,
  watch,
  onBeforeUpdate,
} from "vue";

import swipe from "../directives/swipe.js";
import Tiktok from "./Tiktok.vue";
import TiktoksJson from "../tiktoks.json";

export default defineComponent({
  name: "TikTokStream",
  directives: {
    swipe,
  },
  components: {
    Tiktok,
  },
  setup() {
    const tiktokStream = ref(null);
    const tiktokRefs = ref([]);
    const tiktokData = TiktoksJson;

    const state = reactive({
      currentSlide: 1,
      amoundOfSlides: TiktoksJson.length,
      style: {
        transform: computed(
          () => `translate3d(0, ${-(state.currentSlide - 1) * 100}%, 0)`
        ),
      },
    });

    onBeforeUpdate(() => {
      tiktokRefs.value = [];
    });

    watch(
      () => state.currentSlide,
      (items, oldItems) => {
        tiktokRefs.value[items - 1].play();
        tiktokRefs.value[oldItems - 1].pause();
      },
      {
        lazy: false,
      }
    );

    const onSwipe = (direction) => {
      if (
        (direction === 1 && state.currentSlide === state.amoundOfSlides) ||
        (direction === -1 && state.currentSlide === 1)
      ) {
        return;
      }

      state.currentSlide += direction;
    };

    return {
      state,
      onSwipe,
      tiktokStream,
      tiktokData,
      tiktokRefs,
    };
  },
});
</script>

<style>
.tiktok-stream {
  position: relative;
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
  transform: translate3d(0, 0, 0);
  transition: transform 200ms ease;
}
</style>
