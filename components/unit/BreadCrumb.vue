<template>
  <div class="BreadCrumb">
    <div class="container">
      <nuxt-link :to="localePath('/')"><i class="fas fa-home"></i></nuxt-link>

      <div
        :to="localePath(`/${currentPageLink}`)"
        :class="isSub ? 'subpage' : ''"
        class="main__link"
      >
        / {{ currentPageTranslate }}
      </div>

      <div
        v-if="isSub"
        class="subpage__link"
        :to="localePath(`/${currentPageLink}/${subPage}`)"
      >
        / {{ subPage }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BreadCrumb',
  props: {
    currentPageLink: {
      type: String,
      default: 'Home',
    },
    currentPageTranslate: {
      type: String,
      default: 'Home',
    },
    subPage: {
      type: String,
      default: '',
    },
    isSub: {
      type: Boolean,
      default: false,
    },
  },
}
</script>

<style scoped lang="scss">
@import '~assets/scss/var.scss';

.BreadCrumb {
  margin: 2rem 0;

  .fa-home,
  .main__link,
  .subpage__link {
    display: inline-block;
    color: map-get($colors, accent) !important;
    font-size: 1.2rem;
    font-weight: bold;
    background-color: transparent;
  }

  .subpage {
    color: #5d5d5d !important;
    display: inline-block;
  }
}
</style>
