<template>
  <aside class="sidebar">
    <AdSenseLeftTop />
    <slot name="top"/>
    <SidebarLinks :depth="0" :items="items"/>
    <slot name="bottom"/>
    <div style="text-align: center; margin-top: 5vh;">
      <!-- <a target="_blank" :href="`http://demo.kuboard.cn/#/dashboard?k8sToken=${$site.themeConfig.kuboardToken}`">
        <img style="width: 242px; border: 1px solid #d7dae2;" src="/images/logo-main.png" alt="Kuboard_快速在Kubernetes上落地微服务"/>
      </a> -->

      <p>
        <strong>
          <a target="_blank" :href="`http://demo.kuboard.cn/#/dashboard?k8sToken=${$site.themeConfig.kuboardToken}`">在线体验</a>
        </strong>
      </p>
      <div>
        <a target="_blank" :href="`http://demo.kuboard.cn/#/dashboard?k8sToken=${$site.themeConfig.kuboardToken}`">
          <img :src="demo_img" style="width: 242px; height: 127px; border: 1px solid #d7dae2;" alt="Kubernetes教程：Kuboard在线体验"></img>
        </a>
      </div>
      <p>
      </p>
      <!-- <strong>在线答疑</strong>
      <Qq/>
      <p>
        <img src="/images/kuboard_qq.png" style="width: 120px;" alt="Kubernetes教程：QQ群在线答疑"/>
      </p> -->
    </div>
    <el-divider >Kuboard</el-divider>
    <div style="height: 2rem;"></div>
  </aside>
</template>

<script>
import SidebarLinks from '@theme/components/SidebarLinks.vue'
// import demo from './image-20190723104717575.png'
import demo from './1564841972085.gif'

export default {
  name: 'Sidebar',

  props: ['items'],
  components: { SidebarLinks },
  data () {
    return {
      demo_img: demo
    }
  }

}
</script>

<style lang="stylus">
.sidebar
  ul
    padding 0
    margin 0
    list-style-type none
  a
    display inline-block
  .nav-links
    display none
    border-bottom 1px solid $borderColor
    padding 0.5rem 0 0.75rem 0
    a
      font-weight 600
    .nav-item, .repo-link
      display block
      line-height 1.25rem
      font-size 1.1em
      padding 0.5rem 0 0.5rem 1.5rem
  & > .sidebar-links
    padding 1.5rem 0
    & > li > a.sidebar-link
      font-size 1.1em
      line-height 1.7
      font-weight bold
    & > li:not(:first-child)
      margin-top .75rem

@media (max-width: $MQMobile)
  .sidebar
    .nav-links
      display block
      .dropdown-wrapper .nav-dropdown .dropdown-item a.router-link-active::after
        top calc(1rem - 2px)
    & > .sidebar-links
      padding 1rem 0
</style>
