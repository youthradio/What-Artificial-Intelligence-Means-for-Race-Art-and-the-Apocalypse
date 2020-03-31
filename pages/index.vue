<template>
  <div class="container">
    <HeaderContainer />
    <article>
      <h4>
        {{ articleData.introduction.title }}
      </h4>
      <div
        class="multi-col"
        v-html="articleData.introduction.text"
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
import HeaderContainer from '~/components/Header/HeaderContainer'
import ShareContainer from '~/components/custom/ShareContainer'
import FooterContainer from '~/components/Footer/FooterContainer'

export default {
  components: {
    HeaderContainer,
    ShareContainer,
    FooterContainer
  },
  mixins: [
    CommonUtils
  ],
  asyncData (ctx) {
    return {
      articleData: ArticleData.content
    }
  },
  data () {
    return {
    }
  },
  computed: {},
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
</style>
