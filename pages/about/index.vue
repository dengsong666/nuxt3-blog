<script setup lang="ts">
import { isDev, isPrerender, translateT, useCommonSEOTitle } from "~/utils/nuxt";
import config from "~/config";

useCommonSEOTitle(computed(() => translateT("about")));

const commitSha = computed(() => useRuntimeConfig().app.NUXT_ENV_CURRENT_GIT_SHA);
const commitUrl = computed(() => `https://github.com/${config.githubName}/${config.githubRepo}/commit/${commitSha.value}`);
const buildTime = ref<string>("$(inject:timestamp)");

const paragraphs = [
  "苍穹之下，宇宙以其无尽的疆域昭示着生命的渺小与伟大",
  "我们在其广袤无垠的怀抱中，既是微不足道的尘埃，又是勇敢探索未知的智慧生命，敬畏并挑战着这宏伟的宇宙秩序",
  "宇宙最令人难以理解的就是它的可理解性",
  "我反复说过，在我看来人格化的上帝是幼稚的",
  "你可以称我为不可知论者，但我并不具有专业无神论者那种十字军般的精神，他们的热情大多来自于一种从青年时期接受的宗教教育的束缚中痛苦的挣脱",
  "我更倾向于一种谦卑的态度,以反映我们在智力上对自然和人类本身理解的微弱",
  "如果在我的内心有什么能被称之为宗教的，那就是，对于我们的科学所能够揭示的世界结构，对于这世界结构的无垠敬仰"
];

onBeforeMount(async () => {
  if (!isPrerender) {
    buildTime.value = (!isDev ? (await (await fetch("/timestamp.txt")).text()) : "-");
  }
});
</script>

<template>
  <div class="about flexc">
    <video src="/assets/video/galaxy.mp4" autoplay muted loop />
    <div class="flexc paragraphs">
      <p v-for="p, idx in paragraphs" :key="idx">
        {{ p }}
      </p>
    </div>
    <div class="status">
      Last built &lt;<a target="_blank" :href="commitUrl">{{ commitSha.substring(0, 8) }}</a>&gt; succeeded at
      <time>{{ buildTime }}</time>
    </div>
  </div>
</template>

<style lang="scss">
#default-layout.in-about {
  #header {
    background: rgb(255 255 255 / 5%);
    backdrop-filter: blur(1px);
  }

  #body {
    padding-top: 0;

    .about {
      width: 100vw;
      height: 100vh;
      padding-bottom: $footer-height;
      position: relative;

      video {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        object-fit: cover;
        object-position: right;
        z-index: 1;
      }

      .paragraphs {
        position: relative;
        z-index: 2;
        padding-top: 100px + $header-height;
        justify-content: center;

        p {
          font-size: f-size();
          letter-spacing: 0.8px;
          color: white;
          font-weight: 300;

          &:not(:last-of-type) {
            margin-bottom: 20px;
          }
        }
      }

      .status {
        color: #eee;
        opacity: 0.5;
        font-size: f-size(0.75);
        position: absolute;
        bottom: 10px;
        right: 10px;
        z-index: 1;
        transition: $common-transition;

        &:hover {
          opacity: 1;
        }

        time {
          color: #ffb350;
        }
      }
    }
  }

  #footer {
    position: fixed;
    width: 100%;
    left: 0;
    bottom: 20px;
    z-index: $z-index-footer;

    .middle {
      color: #a1a1a1;
    }

    &:hover {
      .middle {
        color: #e7e7e7;
      }
    }
  }

  #footer,
  #body .about {
    a[href] {
      color: $theme-color;
      transition: $common-transition;

      &:hover {
        color: $theme-color-lighten;
      }
    }
  }
}
</style>
