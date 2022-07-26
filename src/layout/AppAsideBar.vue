<template>
  <ul class="app-aside__bar">
    <!-- 高亮 -->
    <li class="app-aside__bar-slider" :style="sliderStyle"></li>
    <!-- 菜单列表 -->
    <li
      v-for="{ name, routeName, icon } in routeList"
      :key="routeName"
      class="app-aside__bar-item"
      :class="{ active: $route.name === routeName }"
      @click="$router.push({ name: routeName })"
    >
      <!-- icon -->
      <Icon :name="icon" />
      <p>{{ name }}</p>
    </li>
  </ul>
</template>

<script lang="ts">
import { computed, CSSProperties, defineComponent } from 'vue'
import { useRoute } from 'vue-router'
export default defineComponent({
  name: 'AppAsideBar',
  setup() {
    // 路由的实例
    const $route = useRoute()
    // 路由列表
    const routeList = [
      {
        name: '首页',
        routeName: 'Home',
        routePath: 'home',
        icon: 'homefill'
      },
      {
        name: '搜索',
        routeName: 'Search',
        routePath: 'search',
        icon: 'search'
      },
      {
        name: '关于我',
        routeName: 'User',
        routePath: 'user',
        icon: 'user'
      },
      {
        name: 'Pixiv',
        routeName: 'Pixiv',
        routePath: 'pixiv',
        icon: 'iccosplay'
      },
      {
        name: '系统设置',
        routeName: 'Setting',
        routePath: 'setting',
        icon: 'setting1'
      }
    ]
    // 移动动画
    const sliderStyle = computed(() => {
      /**
       *  @grammar array.findIndex(function(currentValue, index, arr), thisValue)
       *
       *  @parmas : 一个函数和对象this
       *
       *  @currentValue 必需。当前元素
       *
       *  @index 可选。当前元素的索引
       *
       *  @arr 可选。当前元素所属的数组对象
       *
       *  @return findIndex() 方法返回传入一个测试条件（函数）符合条件的数组第一个元素位置。
       * */
      const routeIndex = routeList.findIndex((item) =>
        /**
         *  @grammar (array || string).includes(value,index)
         *
         *  @value 必需，要查找的项。
         *
         *  @index 可选，设置从那个位置开始查找，默认为 0。
         *
         *  @return 如果找到匹配的项返回 true，否则返回 false。
         * */
        $route.fullPath.includes(item.routePath)
      )
      return {
        // translate(x,y)	定义 2D 转换。
        // !~ 按二进制位取反
        transform: `translate(
          ${!~routeIndex ? -100 : 0}%,
          ${!~routeIndex ? 0 : routeIndex * 100}%
        )`
      } as CSSProperties
    })
    return {
      routeList,
      sliderStyle
    }
  }
})
</script>
<style lang="less" scoped>
.app-aside__bar {
  @fontSize: 16px;
  @liPadding: 26px;
  @liHeight: 48px;
  position: relative;
  font-size: @fontSize;
  user-select: none;
  &-slider {
    position: absolute;
    top: 0;
    left: 0;
    width: calc(@liPadding + @fontSize*2);
    height: @liHeight;
    background: var(--primary-color);
    z-index: -1;
    border-top-right-radius: 12px;
    border-bottom-right-radius: 12px;
    transition: all 0.25s;
  }
  &-item {
    display: flex;
    align-items: center;
    width: 100%;
    height: @liHeight;
    padding: 0 @liPadding;
    box-sizing: border-box;
    cursor: pointer;
    color: var(--font-unactive-color);
    transition: all 0.25s;

    &.active {
      color: var(--font-color);
      i {
        color: #fff;
      }
    }
    i {
      width: @fontSize;
      height: @fontSize;
      margin-right: @fontSize;
    }
    p {
      line-height: @fontSize;
      margin-left: 16px;
    }
  }
}
</style>
