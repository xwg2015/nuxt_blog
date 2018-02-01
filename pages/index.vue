<template>
  <section class="page page-index" :class="{ 'page-index-loading': loading }">
    <VHeader></VHeader>
    <Intro></Intro>
    <section class="main">
      <div class="card-content article-content">
        <h2>文章</h2>
        <ul class="card-list">
          <li class="card" v-for="(item, index) in articleList" :key="index">
            <a :href="`/article/detail/${item._id}`" class="card-link">
              <div class="img-wrap" v-lazy:background-image="`//xiongwengang.xyz${item.cover}`"></div>
              <h3>{{ item.title }}</h3>
            </a>
          </li>
        </ul>
        <a href="/article/technical" class="more" v-ripple>查看更多</a>
      </div>
      <div class="card-content project-content">
        <h2>项目</h2>
        <ul class="card-list">
          <li class="card" v-for="(item, index) in projectList" v-if="index < 3" :key="index">
            <div class="img-wrap" v-lazy:background-image="`//xiongwengang.xyz${item.cover}`"></div>
            <h3>{{ item.name }}</h3>
          </li>
        </ul>
        <a href="/project/list" class="more" v-ripple>查看更多</a>
      </div>
    </section>
    <VFooter></VFooter>
    <div class="loading" v-if="loading">
      <IndexLoading></IndexLoading>
      <p class="text">总之岁月漫长，然而值得等待...</p>
    </div>
  </section>
</template>

<script>
  import VHeader from '~/components/Header'
  import Intro from '~/components/Intro'
  import VFooter from '~/components/Footer'
  import IndexLoading from '~/components/IndexLoading'
  import axios from 'axios'

  export default {
    components: {
      VHeader,
      Intro,
      VFooter,
      IndexLoading
    },
    asyncData ({ params, error }) {
      return axios.get(`http://admin.xiongwengang.xyz/api/blog/getIndex`).then((res) => {
        return {
          articleList: res.data.articleData,
          projectList: res.data.projectData
        }
      }).catch((e) => {
        error({ statusCode: 404, message: '接口请求报错！' })
      })
    },
    data () {
      return {
        loading: true
      }
    },
    mounted () {
      this.$bus.$on('bg-loaded', () => {
        this.loading = false
      })
    }
  }
</script>

<style lang="sass">
  @import '~assets/sassCore/_function.scss'

  .page-index-loading
    overflow: hidden
  .page-index
    height: 100%
    .loading
      @include display-flex()
      @include flex-direction(column)
      @include justify-content(center)
      @include align-items()
      position: absolute
      left: 0
      top: 0
      width: 100%
      height: 100%
      background-color: #fff
      z-index: 1000
      .text
        margin-top: 10px
    .main
      width: $minWidth
      @include center-block()
    .card-content
      padding: 60px 70px
      margin: 40px 0
      background-color: $white
      border-radius: 4px
      h2
        margin-bottom: 60px
        text-align: center
        font-size: 36px
        color: $themeColor
      .card-list
        @include display-flex()
        @include justify-content()
        @include flex-wrap()
      .card-link
        display: block
      .card-link:hover
        box-shadow: 2px 6px 8px 0 rgba($themeColor, 0.1)
        transition: 1s
      .card
        width: 330px
        margin-bottom: 35px
      .img-wrap
        width: 330px
        height: 220px
        background-size: cover
        background-position: center
        border-radius: 4px 4px 0 0
        overflow: hidden
      .more
        display: block
        width: 330px
        height: 60px
        margin: 45px auto 0
        line-height: 60px
        text-align: center
        font-size: 20px
        color: $white
        background-color: $themeColor
        border-radius: 100px
    .article-content
      h3
        @include display-flex()
        @include align-items()
        height: 76px
        padding: 16px
        font-size: 16px
        color: $themeColor
        border-radius: 0 0 4px 4px
        box-shadow: 0 1px 2px 0 rgba($themeColor, 0.1)
    .project-content
      .card
        position: relative
        border-radius: 4px
        overflow: hidden
        .img-wrap
          transition: 1s
      .card:hover
        .img-wrap
          @include transform(scale(1.2))
      h3
        position: absolute
        left: 40px
        bottom: 12px
        width: 250px
        line-height: 40px
        text-align: center
        font-size: 18px
        color: $white
        background-color: $maskBgColor
        border-radius: 100px
</style>
