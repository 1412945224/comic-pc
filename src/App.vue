<template>
  <div class="app-contain">
    <!-- 头像 与 历史 功能能组件 -->
    <AppTabBar />

    <!-- 主页左侧菜单 -->
    <aside class="app-contain__aside" :class="{ hide: !asideVisible }">
      <!-- 隐藏 与 展开 的开关 -->
      <div
        class="switch"
        :title="asideVisible ? '隐藏' : '展开'"
        @click="asideVisible = !asideVisible"
      ></div>

      <!-- 名称 -->
      <b v-show="asideVisible" class="animate__jello">{{ WEB_NAME }}</b>

      <!-- 菜单列表 -->
      <AppAsideBar v-show="asideVisible" />
    </aside>

    <!-- 右侧内容  -->
    <main class="app-contain__main">
      <AppRouter />
    </main>

    <!-- 动态小人 -->
    <!-- <Live2d
      ref="live2dComp"
      v-model:visible="koharu.visible"
      @click="live2dAction"
    /> -->
  </div>
</template>

<script lang="ts">
import '@/assets/icon/iconfont.css'
import '@/assets/icon/iconfont.js'
import { defineComponent, ref } from 'vue' // onMounted,

// import Live2d from '@/components/Live2d/Live2d.vue'
import AppAsideBar from '@/layout/AppAsideBar.vue'
import AppRouter from '@/layout/AppRouter.vue'
import AppTabBar from '@/layout/AppTabBar.vue'

import { WEB_NAME } from './common/static'
import { useIsDev } from './hooks/utils'
import { useSystemConfigStore } from './stores/systemConfig.store'
// import { useKoharu } from '@/stores/koharu.store'

function provideModule() {
  const { isDev } = useIsDev()
  return {
    isDev
  }
}
function asideModule() {
  const asideVisible = ref(true)
  return {
    asideVisible
  }
}
// 动态小人

// function live2dModule() {
//   const live2dComp = ref<InstanceType<typeof Live2d>>()
//   const koharu = useKoharu()

//   const live2dAction = () => {
//     live2dComp.value?.motion('else')
//   }
//   onMounted(() => {
//     live2dComp.value?.initLive2d({
//       model: '/live2dModels/koharu/koharu.model.json',
//       version: 2,
//       size: {
//         width: 210,
//         height: 260
//       }
//     })
//   })
//   return {
//     live2dComp,
//     live2dAction,
//     koharu
//   }
// }

export default defineComponent({
  name: 'Comic',
  components: {
    AppAsideBar,
    AppRouter,
    AppTabBar
    // Live2d
  },
  setup() {
    const systemConfigStore = useSystemConfigStore()
    systemConfigStore.init()
    return {
      WEB_NAME,
      ...asideModule(),
      ...provideModule()
      // ...live2dModule()
    }
  }
})
</script>

<style lang="less">
@import '~styles/common';
@import '~styles/app';
</style>

<style lang="less" scoped>
@import '~styles/var';
.app-contain {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  background: var(--bg-color);
  color: var(--font-color);
  @slideDruation: 0.625s;
  &__aside {
    display: flex;
    flex-direction: column;
    width: 280px;
    height: calc(100% - @frameTop*2);
    background: var(--aside-bg-color);
    border-radius: var(--df-radius);
    padding-left: @frameTop;
    box-sizing: border-box;
    animation: slide-in @slideDruation forwards;
    transition: all 0.25s;
    &.hide {
      width: 80px;
    }
    & > b {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 100%;
      aspect-ratio: 2/1;
      font-size: 30px;
      animation-duration: 1.25s;
      animation-delay: @slideDruation;
    }
    .switch {
      position: absolute;
      top: 0;
      right: 8px;
      bottom: 0;
      margin: auto 0;
      width: 20px;
      height: 100px;
      cursor: pointer;
      &::before {
        .mask(1,var(--font-color));
        right: 0;
        margin: 0 auto;
        width: 5px;
        height: 100%;
        border-radius: 20px;
        opacity: 0.6;
        transition: all 0.25s;
      }
      &:hover::before {
        opacity: 0.8;
      }
    }
    @keyframes slide-in {
      from {
        transform: translateX(-100%);
      }
      to {
        transform: translateX(-@frameTop);
      }
    }
  }
  &__main {
    margin-top: @frameTop;
    box-sizing: border-box;
    height: calc(100% - @frameTop);
    flex: 1;
    overflow: hidden;
    position: relative;
    opacity: 0;
    animation: fade-in 1s @slideDruation forwards;
    @keyframes fade-in {
      to {
        opacity: 1;
      }
    }
  }
}
</style>
