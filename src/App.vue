<template>
  <video ref="cameraTrack" class="video-output"></video>
</template>

<script lang="ts">
import { defineComponent } from "vue";

import { createLocalVideoTrack, createLocalAudioTrack } from "livekit-client";
import { BackgroundBlur } from "@livekit/track-processors";

export default defineComponent({
  name: "test",
  data() {
    return {};
  },
  mounted() {
    this.startVideo();
  },
  methods: {
    async startVideo() {
      try {
        let videoElement = this.$refs.cameraTrack as HTMLMediaElement;

        let videoTrack = await createLocalVideoTrack({
          // @ts-expect-error
          resolution: {
            aspectRatio: 16 / 9,
          },
        });

        await videoTrack.setProcessor(BackgroundBlur(10));

        videoTrack.attach(videoElement);
      } catch (error) {
        console.warn(error);
      }
    },
  },
});
</script>

<style scoped>
.video-output {
  width: 30rem;
  border: 1px solid;
}
</style>
