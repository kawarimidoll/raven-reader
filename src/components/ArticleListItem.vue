<template>
  <div
    class="artcle-list-item"
    mark="article"
    :class="{ active: isArticleActive(article.articles) }"
    @click="setActiveArticleId(article.articles)"
    @contextmenu.prevent="openArticleContextMenu($event, { article: article.articles })"
  >
    <router-link
      :to="`/article/${article.id}`"
      :class="{ 'article-read': article.articles.read }"
      class="list-group-item list-group-item-action flex-column align-items-start"
    >
      <div class="d-flex flex-column w-100">
        <p class="mb-1">
          <small><img
            v-if="article.feeds.favicon"
            :src="article.feeds.favicon"
            width="16"
            height="16"
            class="mr-2"
          > {{ article.feeds.title }}</small>
        </p>
        <p class="mb-2">
          <small>{{ article.formatDate }}</small>
        </p>
      </div>
      <h6><strong>{{ article.articles.title }}</strong></h6>
      <p>{{ article.articles.contentSnippet }}</p>
      <p
        v-if="article.articles.favourite"
        class="text-right mb-0"
      >
        <feather-icon
          name="star"
          size="sm"
          :filled="article.articles.favourite"
        />
      </p>
    </router-link>
  </div>
</template>
<script>
export default {
  props: {
    article: {
      type: Object,
      default: null
    }
  },
  computed: {
    activeArticleId () {
      return this.$store.getters.activeArticleId
    }
  },
  methods: {
    openArticleContextMenu (e, article) {
      window.electron.createContextMenu('article', article)
    },
    setActiveArticleId (article) {
      return this.$store.dispatch('setActiveArticleId', article)
    },
    isArticleActive (article) {
      return !!article && article.uuid !== undefined && article.uuid === this.activeArticleId
    }
  }
}
</script>
<style lang="scss">
.artcle-list-item {
  -webkit-user-select: none;
  &.active {
    background-color: var(--active-item-background-color);
    border-radius: 0.3rem;
  }

  h6 {
    line-height: 1.5;
  }
}
</style>
