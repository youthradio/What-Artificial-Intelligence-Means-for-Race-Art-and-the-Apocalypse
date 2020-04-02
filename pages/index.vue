<template>
  <div class="container">
    <FeatureHeaderText
      :header-data="headerData"
      @onHeaderImgHeight="headerImageHeight = $event"
    />
    <MenuHeader :offset="headerImageHeight" />

    <article>
      <h4>
        {{ articleData.introduction.title }}
      </h4>
      <div
        class="multi-col"
        v-html="articleData.introduction.text"
      />
    </article>

    <article
      v-for="(section, index) in articleData.sections"
      :key="section.title"
    >
      <h5> Section {{ index + 1 }}</h5>
      <h1>
        {{ section.title }}
      </h1>
      <div
        class="multi-col"
        v-html="section.text"
      />
      <VoiceDialog
        :audios="section.audio"
        :dialogs="tracks.get(section.key)"
        :guests="articleData.guests"
      />
    </article>
    <h4>
      {{ articleData.about.title }}
    </h4>
    <div
      class="multi-col"
      v-html="articleData.about.text"
    />
    <ShareContainer />
    <FooterContainer />
  </div>
</template>

<script>

import CommonUtils from '../mixins/CommonUtils'
import ArticleData from '../data/data.json'
import Tracks from '../data/tracks.json'

import POSTCONFIG from '~/post.config'
import FeatureHeaderText from '~/components/Header/FeatureHeaderText'
import MenuHeader from '~/components/Header/MenuHeader'
import ShareContainer from '~/components/Custom/ShareContainer'
import FooterContainer from '~/components/Footer/FooterContainer'
import VoiceDialog from '~/components/Custom/VoiceDialog.vue'

export default {
  components: {
    MenuHeader,
    FeatureHeaderText,
    ShareContainer,
    FooterContainer,
    VoiceDialog
  },
  mixins: [
    CommonUtils
  ],
  asyncData (ctx) {
    return {
      articleData: ArticleData.content,
      tracks: new Map(Tracks)
    }
  },
  data () {
    return {
      headerImageHeight: 0
    }
  },
  computed: {
    headerData () {
      return {
        featureImage: POSTCONFIG.featureImagePath,
        title: this.articleData.title,
        subtitle: this.articleData.subtitle,
        suptitle: this.articleData.suptitle,
        author: POSTCONFIG.author,
        imageCaption: POSTCONFIG.featureImageCaption,
        publishDate: POSTCONFIG.publishDate,
        location: POSTCONFIG.location
      }
    }
  },
  watch: {

  },
  mounted () {
  },
  methods: {

  }
}
</script>

<style lang="scss" scoped>
@import "~@/css/vars";
@import "~@/css/base";
@import "~@/css/mixins";

.multi-col {
  column-count: 1;

  @include breakpoint(medium) {
    column-count: 2;
  }
}
h1 {
  padding-top: 0;
  font-weight: 800;
  font-family: "Assistant";
  font-size: 3rem;
  @include breakpoint(medium) {
    font-size: 5rem;
  }
}
h5 {
  padding: 0rem;
}
</style>
