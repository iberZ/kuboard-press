<template>
  <main class="page">
    <div class="page-nav" style="max-width: 1000px; margin-top: 56px;">
      <AdSensePageTop></AdSensePageTop>
    </div>
    <!-- <AdsenseParagraph></AdsenseParagraph> -->
    <slot name="top"/>

    <Content class="theme-default-content" style="padding-top: 0; margin-top: 0; padding-bottom: 1rem;"/>

    <div>
    </div>

    <!-- <div style="margin: auto; width: 182px;">
      <p>
        <Qq/> 在线答疑
      </p>
      <p>
        <img src="/images/kuboard_qq.png" alt="Kubernetes教程：QQ群在线答疑"/>
      </p>
    </div> -->
<div class="page-nav" style="max-width: 1000px; padding: 1rem;">
      <el-divider>在线答疑</el-divider>
  <grid :rwd="{compact: 'stack'}">
    <grid-item size="1/3" :rwd="{tablet: '1/1', compact: '1/1'}" >
      <el-card style="height: 100%; margin-top: 1rem;" shadow="none" :body-style="{padding: '0rem 1.5rem'}">
        <h3>QQ群（免费）</h3>
        <div>
          <Qq/> 808894550
        </div>
        <p style="margin-bottom: 0;">
          <img style="margin: auto; padding: 7px;" src="/images/kuboard_qq.png" alt="Kubernetes教程：QQ群在线答疑"/>
        </p>
      </el-card>
    </grid-item>
    <grid-item size="2/3" :rwd="{tablet: '1/1', compact: '1/1'}">
        <el-card style="height: 100%; color: #2c3e50; line-height: 1.7; margin-top: 1rem;" shadow="none" :body-style="{padding: '0rem 1.5rem'}">
          <h3>微信群（即时）</h3>
          <div>
            <div style="margin-top: 10px;">
              <span>扫第一个二维码完成打赏，扫第二个加微信进群聊（请发送打赏截图）</span>
              <p style="margin-top: 10px; margin-bottom: 0;">
                <img src="/images/dz.png" style="width: 200px; margin-right: 120px;"></img>
                <img src="/images/dz2.jpeg" style="float: right; width: 200px;"></img>
              </p>
            </div>
          </div>
        </el-card>
    </grid-item>
  </grid>
</div>


    <div class="page-nav" style="max-width: 1000px; padding-top:0; margin-top: 1rem;">
      <AdSensePageBottomInline/>
    </div>
    <!-- <div style="text-align: center; margin-bottom: 10px;" v-if="$page.path.indexOf('/learning/') === 0">
      <a href="https://github.com/eip-work/kuboard-press" target="_blank">如果您觉得 Kubernetes教程 有帮到您，点击此处，给个 Github Star，谢谢！</a>
    </div>
    <div style="text-align: center; margin-bottom: 10px;" v-else>
      <a href="https://github.com/eip-work/kuboard-press" target="_blank">如果您觉得这篇文档有帮到您，点击此处，给个 Github Star，谢谢！</a>
    </div> -->
    <!-- <Valine></Valine> -->
    <footer class="page-edit" style="max-width: 1000px;">
      <div
        class="edit-link"
        v-if="editLink"
      >
        <a
          :href="editLink"
          target="_blank"
          rel="noopener noreferrer"
        >{{ editLinkText }}</a>
        <OutboundLink/>
      </div>

      <div
        class="last-updated"
        v-if="lastUpdated"
      >
        <span class="prefix">{{ lastUpdatedText }}: </span>
        <span class="time">{{ lastUpdated }}</span>
      </div>
    </footer>

    <div class="page-nav" style="max-width: 1000px;" v-if="prev || next">
      <p class="inner">
        <span
          v-if="prev"
          class="prev"
        >
          ←
          <router-link
            v-if="prev"
            class="prev"
            :to="prev.path"
          >
            {{ prev.title || prev.path }}
          </router-link>
        </span>

        <span
          v-if="next"
          class="next"
        >
          <router-link
            v-if="next"
            :to="next.path"
          >
            {{ next.title || next.path }}
          </router-link>
          →
        </span>
      </p>
    </div>
    <div>
    </div>
    <PageVssue class="theme-default-content" style="max-width: 1000px; margin-top: -1.6rem; padding-top: 0;"></PageVssue>
    <slot name="bottom"/>
    <AdSenseRightSide/>
  </main>
</template>

<script>
import { resolvePage, outboundRE, endingSlashRE } from '../util'
import PageVssue from './PageVssue'

export default {
  props: ['sidebarItems'],
  components: { PageVssue },
  computed: {
    lastUpdated () {
      return this.$page.lastUpdated
    },

    lastUpdatedText () {
      if (typeof this.$themeLocaleConfig.lastUpdated === 'string') {
        return this.$themeLocaleConfig.lastUpdated
      }
      if (typeof this.$site.themeConfig.lastUpdated === 'string') {
        return this.$site.themeConfig.lastUpdated
      }
      return 'Last Updated'
    },

    prev () {
      const prev = this.$page.frontmatter.prev
      if (prev === false) {
        return
      } else if (prev) {
        return resolvePage(this.$site.pages, prev, this.$route.path)
      } else {
        return resolvePrev(this.$page, this.sidebarItems)
      }
    },

    next () {
      const next = this.$page.frontmatter.next
      if (next === false) {
        return
      } else if (next) {
        return resolvePage(this.$site.pages, next, this.$route.path)
      } else {
        return resolveNext(this.$page, this.sidebarItems)
      }
    },

    editLink () {
      if (this.$page.frontmatter.editLink === false) {
        return
      }
      const {
        repo,
        editLinks,
        docsDir = '',
        docsBranch = 'master',
        docsRepo = repo
      } = this.$site.themeConfig

      if (docsRepo && editLinks && this.$page.relativePath) {
        return this.createEditLink(repo, docsRepo, docsDir, docsBranch, this.$page.relativePath)
      }
    },

    editLinkText () {
      return (
        this.$themeLocaleConfig.editLinkText
        || this.$site.themeConfig.editLinkText
        || `Edit this page`
      )
    }
  },

  mounted () {
    window.openOutboundLink = function (event) {
      // console.log('openOutboundLink', event.text, new URL(event.href))
      let e = {
        hitType: 'event',
        eventCategory: 'OL:' + new URL(event.href).host,
        eventAction: 'OL:' + event.href,
        eventLabel: 'OL:' + event.text
      }
      if (window.ga) {
        window.ga('send', e);
        // console.log('openOutboundLink Event', e)
      } else {
        console.log('开发环境，不发送 ga event', e)
      }
    }
  },

  methods: {
    createEditLink (repo, docsRepo, docsDir, docsBranch, path) {
      const bitbucket = /bitbucket.org/
      if (bitbucket.test(repo)) {
        const base = outboundRE.test(docsRepo)
          ? docsRepo
          : repo
        return (
          base.replace(endingSlashRE, '')
           + `/src`
           + `/${docsBranch}/`
           + (docsDir ? docsDir.replace(endingSlashRE, '') + '/' : '')
           + path
           + `?mode=edit&spa=0&at=${docsBranch}&fileviewer=file-view-default`
        )
      }

      const base = outboundRE.test(docsRepo)
        ? docsRepo
        : `https://github.com/${docsRepo}`
      return (
        base.replace(endingSlashRE, '')
        + `/edit`
        + `/${docsBranch}/`
        + (docsDir ? docsDir.replace(endingSlashRE, '') + '/' : '')
        + path
      )
    }
  }
}

function resolvePrev (page, items) {
  return find(page, items, -1)
}

function resolveNext (page, items) {
  return find(page, items, 1)
}

function find (page, items, offset) {
  const res = []
  flatten(items, res)
  for (let i = 0; i < res.length; i++) {
    const cur = res[i]
    if (cur.type === 'page' && cur.path === decodeURIComponent(page.path)) {
      return res[i + offset]
    }
  }
}

function flatten (items, res) {
  for (let i = 0, l = items.length; i < l; i++) {
    if (items[i].type === 'group') {
      flatten(items[i].children || [], res)
    } else {
      res.push(items[i])
    }
  }
}

</script>

<style lang="stylus">
@require '../styles/wrapper.styl'

.page
  padding-bottom 2rem
  display block

.page-edit
  @extend $wrapper
  padding-top 1rem
  padding-bottom 1rem
  overflow auto
  .edit-link
    display inline-block
    a
      color lighten($textColor, 25%)
      margin-right 0.25rem
  .last-updated
    float right
    font-size 0.9em
    .prefix
      font-weight 500
      color lighten($textColor, 25%)
    .time
      font-weight 400
      color #aaa

.page-nav
  @extend $wrapper
  padding-top 1rem
  padding-bottom 0
  .inner
    min-height 2rem
    margin-top 0
    border-top 1px solid $borderColor
    padding-top 1rem
    overflow auto // clear float
  .next
    float right

@media (max-width: $MQMobile)
  .page-edit
    .edit-link
      margin-bottom .5rem
    .last-updated
      font-size .8em
      float none
      text-align left

</style>
