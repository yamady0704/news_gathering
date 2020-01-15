<template>
  <div class="md-layout md-alignment-center" style="margin: 4em 0">
    <!-- Top Navigation -->
    <md-toolbar class="fixed-toolbar" elevation="1">
      <md-button class="md-icon-button">
        <md-icon>menu</md-icon>
      </md-button>
      <nuxt-link class="md-primary md-title" to="/">
        NuxtNews
      </nuxt-link>

      <div class="md-toolbar-section-end">
        <md-button to="/login">ログイン</md-button>
        <md-button to="/resister">登録</md-button>
        <md-button class="md-accent" @click="showSidepanel = true">カテゴリ</md-button>
      </div>
    </md-toolbar>

    <!-- News Categories (Right Drawn) -->
    <md-drawer class="md-right" md-fixed :md-active.sync="showSidepanel">
      <md-toolbar :md-elecation="1">
        <span class="md-title">ニュースカテゴリ</span>
      </md-toolbar>

      <md-progress-bar v-if="loading" md-mode="indeterminate"></md-progress-bar>

      <md-list>
        <md-subheader class="md-primary">カテゴリ一覧</md-subheader>
        <md-list-item v-for="(newsCategory, i) in newsCategories" :key="i"
        @click="loadCategory(newsCategory.path)">
          <md-icon :class="newsCategory.path === category ? 'md-primary' : ''">{{newsCategory.icon}}
          </md-icon>
          <span class="md-list-item-text">{{newsCategory.name}}</span>
        </md-list-item>
      </md-list>
    </md-drawer>
    <!-- App Content -->
    <div class="md-layout-item md-size-95">
      <md-content class="md-layout md-gutter" style="background: #007998; padding: 1em;">
        <ul v-for="headline in headlines" :key="headline.id" class="md-layout-item
        md-large-size-25 md-medium-size-33 md-small-size-50 md-xsmall-size-100">
          <md-card style="margin-top: 1em;" md-with-hover>
            <md-ripple>
            <md-card-media md-ratio="16:9">
              <img :src="headline.urlToImage" :alt="headline.title">
            </md-card-media>

            <md-card-header>
              <div class="md-card-title">
                <a :href="headline.url" target="_blank">{{headline.title}}</a>
              </div>
              <div>
                {{headline.source.name}}
                <md-icon class="small-icon">book</md-icon>
              </div>
              <div class="md-subhead" v-if="headline.author">
                {{headline.author}}
                <md-icon class="small-icon">face</md-icon>
              </div>
              <div class="md-subhead">
                {{headline.publishedAt}}
                <md-icon class="small-icon">alarm</md-icon>
              </div>
            </md-card-header>

            <md-card-content>{{headline.description}}</md-card-content>
            <md-card-actions>
              <md-button class="md-icon-button">
                <md-icon>bookmark</md-icon>
              </md-button>
              <md-button class="md-icon-button">
                <md-icon>message</md-icon>
              </md-button>
            </md-card-actions>
            </md-ripple>
          </md-card>
        </ul>
      </md-content>
    </div>
  </div>
</template>

<script>
  export default {
    data: () => ({
      showSidepanel: false,
      newsCategories: [
        { name: 'トップニュース', path: '', icon: 'today' },
        { name: 'テクノロジー', path: 'technology', icon: 'keyboard' },
        { name: 'ビジネス', path: 'business', icon: 'business_center' },
        { name: 'エンターテイメント', path: 'entertainment', icon: 'weekend' },
        { name: 'サイエンス', path: 'science', icon: 'fingerprint' },
        { name: 'スポーツ', path: 'sports', icon: 'golf_course' }
      ]
    }),
    async fetch({ store }) {
      await store.dispatch('loadHeadlines', `/api/top-headlines?country=jp&category=${store.state.category}`);
    },
    computed: {
      headlines() {
        return this.$store.getters.headlines;
      },
      category() {
        return this.$store.getters.category;
      },
      loading() {
        return this.$store.getters.loading;
      }
    },
    methods: {
      async loadCategory(category) {
        this.$store.commit('setCategory', category);
        await this.$store.dispatch('loadHeadlines', `/api/top-headlines?country=jp&category=${this.category}`);
      }
    }
  }
</script>

<style scoped>
  .small-icon {
    font-size: 18px !important;
  }

  .fixed-toolbar {
    position: fixed;
    top: 0;
    z-index: 5;
  }
</style>