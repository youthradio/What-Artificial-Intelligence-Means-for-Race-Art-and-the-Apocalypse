<template>
  <div
    tabindex="0"
    class="container"
  >
    <FeatureHeaderText
      :header-data="headerData"
      @onHeaderImgHeight="headerImageHeight = $event"
    />
    <MenuHeader :offset="headerImageHeight" />

    <article class="top-margin">
      <h4 id="introduction">
        {{ articleData.introduction.title }}
      </h4>
      <div
        class="multi-col top-margin"
        v-html="articleData.introduction.text"
      />
    </article>
    <div class="section-table-contents">
      <div class="global-margin section-menu-header">
        <div class="menu-col-l">
          <a
            v-for="(section, index) in articleData.sections.slice(0,3)"
            :key="section.title + section.subtitle"
            class="menu-row"
            href="#"
            @click.prevent="jumpToId(`#section-${index+1}`)"
          >
            <h5> Section {{ index + 1 }}</h5>
            <h3>
              {{ section.title }}
            </h3>
          </a>
        </div>
        <div class="menu-col-r">
          <a
            v-for="(section, index) in articleData.sections.slice(3)"
            :key="section.title + section.subtitle"
            class="menu-row"
            href="#"
            @click.prevent="jumpToId(`#section-${index+4}`)"
          >
            <h5> Section {{ index + 4 }}</h5>
            <h3>
              {{ section.title }}
            </h3>
            <h4 v-if="section.subtitle">
              {{ section.subtitle }}
            </h4>
          </a>
          <a
            class="menu-row no-padding"
            href="#"
            @click.prevent="jumpToId('#about')"
          >
            <h5> {{ articleData.about.title }} </h5>
          </a>
          <a
            class="menu-row no-padding"
            href="#"
            @click.prevent="jumpToId('#credits')"
          >
            <h5>{{ articleData.credits.title }} </h5>
          </a>
        </div>
      </div>
    </div>
    <div>
      <div class="section-menu-header-container">
        <div
          class="relative-top"
          @click.prevent="toggleMenu"
        >
          <span :class="['close-icon', !bmenuSections? 'icon-arrow-bold-down': 'icon-close']" />

          <div class="global-margin section-menu-fixed">
            <h5> Section {{ activeSection + 1 }}</h5>
            <div class="margin-left">
              <h3>
                {{ activeSectionData.title }}
              </h3>
              <h4 v-if="activeSectionData.subtitle">
                {{ activeSectionData.subtitle }}
              </h4>
            </div>
          </div>
        </div>
        <div
          v-if="bmenuSections"
          class="absolute-top"
        >
          <div class="global-margin section-menu-header">
            <div class="menu-col-l">
              <a
                v-for="(section, index) in articleData.sections.slice(0,3)"
                :key="section.title + section.subtitle"
                class="menu-row"
                href="#"
                @click.prevent="jumpToId(`#section-${index+1}`)"
              >
                <h5> Section {{ index + 1 }}</h5>
                <div class="margin-left">
                  <h3>
                    {{ section.title }}
                  </h3>
                </div>
              </a>
            </div>
            <div class="menu-col-r">
              <a
                v-for="(section, index) in articleData.sections.slice(3)"
                :key="section.title + section.subtitle"
                class="menu-row"
                href="#"
                @click.prevent="jumpToId(`#section-${index+4}`)"
              >
                <h5> Section {{ index + 4 }}</h5>
                <div class="margin-left">
                  <h3>
                    {{ section.title }}
                  </h3>
                  <h4 v-if="section.subtitle">
                    {{ section.subtitle }}
                  </h4>
                </div>
              </a>
              <a
                class="menu-row no-padding"
                href="#"
                @click.prevent="jumpToId('#introduction')"
              >
                <h5> {{ articleData.introduction.title }}</h5>
              </a>
              <a
                class="menu-row no-padding"
                href="#"
                @click.prevent="jumpToId('#about')"
              >
                <h5> {{ articleData.about.title }} </h5>
              </a>
              <a
                class="menu-row no-padding"
                href="#"
                @click.prevent="jumpToId('#credits')"
              >
                <h5>{{ articleData.credits.title }} </h5>
              </a>
            </div>
          </div>
        </div>
      </div>
      <article>
        <div
          v-for="(section, index) in articleData.sections"
          :key="section.title + section.subtitle"
          ref="section"
          :data-section-id="index"
          class="section top-margin"
        >
          <h5 :id="`section-${index + 1}`">
            Section {{ index + 1 }}
          </h5>
          <h1>
            {{ section.title }}
          </h1>
          <h2 v-if="section.subtitle">
            {{ section.subtitle }}
          </h2>
          <div
            class="multi-col top-margin"
            v-html="section.text"
          />
          <template v-if="section.audio.length">
            <VoiceDialog
              class="top-margin"
              :audios="section.audio"
              :dialogs="tracks.get(section.key)"
              :guests="articleData.guests"
            />
          </template>
          <template v-if="section.dialogue && section.dialogue.length">
            <TextDialog
              class="top-margin"
              :dialogs="section.dialogue"
            />
          </template>
          <template v-if="section.quiz">
            <PollComponent :question-set="section.quiz" />
          </template>
        </div>
      </article>
    </div>
    <article>
      <h5 id="about">
        {{ articleData.about.title }}
      </h5>
      <div
        class="multi-col top-margin"
        v-html="articleData.about.text"
      />
      <h5 id="credits">
        {{ articleData.credits.title }}
      </h5>
      <div
        class="top-margin"
      >
        <div
          v-for="person in articleData.credits.people"
          :key="person.names"
          class="credits"
        >
          <span> {{ person.title }}: </span> {{ person.names }}
        </div>
      </div>
    </article>

    <ShareContainer />
    <FooterMenu />
    <FooterEmailSubscribe />
  </div>
</template>

<script>

import CommonUtils from '../mixins/CommonUtils'
import ArticleData from '../data/data.json'
import Tracks from '../data/tracks.json'
import FooterEmailSubscribe from '~/components/Footer/FooterEmailSubscribe'
import FooterMenu from '~/components/Footer/FooterMenu'
import POSTCONFIG from '~/post.config'
import FeatureHeaderText from '~/components/Header/FeatureHeaderText'
import MenuHeader from '~/components/Header/MenuHeader'
import ShareContainer from '~/components/Custom/ShareContainer'
import VoiceDialog from '~/components/Custom/VoiceDialog.vue'
import TextDialog from '~/components/Custom/TextDialog.vue'
import PollComponent from '~/components/Custom/PollComponent'

export default {
  components: {
    MenuHeader,
    FooterEmailSubscribe,
    FooterMenu,
    FeatureHeaderText,
    ShareContainer,
    VoiceDialog,
    TextDialog,
    PollComponent
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
      headerImageHeight: 0,
      bmenuSections: false,
      activeSection: 0
    }
  },
  computed: {
    activeSectionData () {
      return this.articleData.sections[this.activeSection]
    },
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
    const observer = new IntersectionObserver((entries, observer) => {
      entries.forEach((entry) => {
        if (entry.intersectionRatio >= 0.3) {
          this.activeSection = +entry.target.dataset.sectionId
        }
      })
    }, { threshold: [0.3] })
    this.$refs.section.map(section => observer.observe(section))
  },
  methods: {
    toggleMenu () {
      this.bmenuSections = !this.bmenuSections
    },
    jumpToId (id) {
      this.$el.querySelector(id).scrollIntoView(true)
      this.bmenuSections = false
    }
  }
}
</script>

<style lang="scss" scoped>
@import "~@/css/vars";
@import "~@/css/base";
@import "~@/css/mixins";
a:hover {
  background-color: $lightpurple;
}
.multi-col {
  column-count: 1;

  @include breakpoint(medium) {
    column-count: 2;
  }
}
.container {
  h1 {
    padding-top: 0;
    padding-bottom: 0;
    font-weight: 800;
    font-size: 3rem;
    @include breakpoint(medium) {
      font-size: 5rem;
    }
  }
  h2 {
    padding-top: 0;
    padding-bottom: 0;
  }

  h4,
  h5 {
    padding: 0rem;
    &[id]::before {
      content: "";
      display: block;
      height: 130px;
      margin-top: -130px;
      visibility: hidden;
    }
  }
}
.global-margin {
  position: relative;
  max-width: 35rem;
  padding-left: 1rem;
  padding-right: 1rem;
  margin-left: auto;
  margin-right: auto;
}
.section-menu-header-container {
  position: sticky;
  z-index: 20;
  background-color: $darkblue;
  color: $white;
  top: 68px;
  a {
    color: $white;
    border-bottom: unset;
  }
  h3,
  h4,
  h5 {
    padding: 0;
  }
  h5 {
    display: inline;
    font-size: 0.2rem;
    @include breakpoint(medium) {
      font-size: 0.5rem;
    }
  }
  h4 {
    font-size: 0.3rem;
    @include breakpoint(medium) {
      font-size: 0.6rem;
      font-weight: bold;
    }
  }
  h3 {
    font-size: 0.9rem;
    @include breakpoint(medium) {
      font-size: 1rem;
    }
  }
  .absolute-top {
    position: absolute;
    top: 100%;
    width: 100%;
    background-color: $darkblue;
  }
  .close-icon {
    cursor: pointer;
    position: absolute;
    right: 0;
    margin-right: 35px;
    font-size: 0.6em;
    top: 50%;
    transform: translateY(-50%);
  }
  .section-menu-fixed {
    display: flex;
    align-items: center;
  }
}

.section-menu-header {
  h3,
  h4,
  h5 {
    color: $white;
    padding: 0;
  }
  @include breakpoint(medium) {
    display: grid;
    width: 100%;
    grid-template-columns: 1fr 1fr;
    grid-template-areas: "left right";
    padding-left: 1rem;
    padding-right: 1rem;
  }
  .menu-row {
    display: flex;
    padding-bottom: 1rem;
    // align-items: center;
  }
  .menu-col-l {
    grid-area: left;
  }
  .menu-col-r {
    grid-area: right;
  }
}
.section-table-contents {
  background-color: $grey;
  height: 100vh;
  background: radial-gradient(circle at 25% 10%, #292e49 20%, transparent 100%),
    radial-gradient(circle at 70% 30%, #6b5b67 50%, transparent 100%),
    radial-gradient(at 30% 50%, #332849 20%, transparent 100%),
    radial-gradient(at 70% 90%, #18102c 20%, white 100%);

  min-height: 60vh;
  display: flex;
  place-items: center;
  .menu-row {
    display: block;
    padding-bottom: 1rem;
  }
  a {
    border-bottom: unset;
  }
}
.top-margin {
  margin-top: 1rem;
}
.no-padding {
  padding-bottom: 0 !important;
}
.margin-left {
  margin-left: 0.5rem;
}
.credits{
  font-size: 0.8rem;
  span {
    font-weight: 800;
  }
}
</style>
