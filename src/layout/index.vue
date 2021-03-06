<template>
  <div :class="classObj" class="app-wrapper">
    <div v-if="device==='mobile'&&sidebar.opened" class="drawer-bg" @click="handleClickOutside" />
    <Sidebar class="sidebar-container" />
    <div class="main-container">
      <div :class="{'fixed-header':fixedHeader}">
        <Navbar />
        <TagsView v-if="needTagsView" />
        <div style="float:right">
          <GithubCorner />
        </div>
      </div>
      <div :class="{'margin-app':fixedHeader}">
        <AppMain />
      </div>
      <RightPanel v-if="showSettings" :button-top="165" :el-icon="'el-icon-setting'">
        <Settings />
      </RightPanel>
      <BottomDrawer />
    </div>
  </div>
</template>

<script>
import RightPanel from '@/components/RightPanel'
import BottomDrawer from '@/components/BottomDrawer'

import GithubCorner from '@/components/GithubCorner'
import {
  Navbar,
  Sidebar,
  AppMain,
  Settings,
  TagsView
} from './components'
import ResizeMixin from './mixin/ResizeHandler'
import {
  mapGetters
} from 'vuex'

export default {
  name: 'Layout',
  components: {
    Navbar,
    Sidebar,
    AppMain,
    Settings,
    GithubCorner,
    RightPanel,
    BottomDrawer,
    TagsView
  },
  mixins: [ResizeMixin],
  computed: {
    ...mapGetters([
      'sidebar',
      'device',
      'showSettings',
      'needTagsView',
      'fixedHeader'
    ]),
    classObj() {
      return {
        hideSidebar: !this.sidebar.opened,
        openSidebar: this.sidebar.opened,
        withoutAnimation: this.sidebar.withoutAnimation,
        mobile: this.device === 'mobile'
      }
    }
  },
  methods: {
    handleClickOutside() {
      this.$store.dispatch('app/closeSideBar', {
        withoutAnimation: false
      })
    }
  }
}

</script>

<style lang="scss" scoped>
  @import "~@/styles/mixin.scss";
  @import "~@/styles/variables.scss";

  .app-wrapper {
    @include clearfix;
    position: relative;
    height: 100%;
    width: 100%;

    &.mobile.openSidebar {
      position: fixed;
      top: 0;
    }
  }

  .drawer-bg {
    background: #303133;
    opacity: 0.5;
    width: 100%;
    top: 0;
    height: 100%;
    position: absolute;
    z-index: 999;
  }

  .fixed-header {
    position: fixed;
    top: 0;
    right: 0;
    z-index: 9;
    width: calc(100% - #{$sideBarWidth});
    transition: width 0.28s;
  }

  .margin-app {
    margin-top: 100px
  }

  .hideSidebar .fixed-header {
    width: calc(100% - 54px)
  }

  .mobile .fixed-header {
    width: 100%;
  }

</style>
