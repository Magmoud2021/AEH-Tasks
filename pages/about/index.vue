<template>
  <section class="about-page">
    <TopPhoto :bg-img="Setting.about_us_header_image" />
     <AboutUs />
    <OurBranches :bg-img="Setting.about_us_header_image" />
    <AboutStatus /> -->
  </section>
</template>

<script>
import { mapGetters } from 'vuex'
import { fetchStore } from '~/mixin/fetchStore'
export default {
  name: 'AboutPage',
  layout: 'DefaultLayout',
  computed: {
    ...mapGetters(['Setting']),
  },
  mixins: [
    fetchStore([
      {
        stateName: 'contactUs',
      },
    ]),
  ],
  head() {
    return {
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.contactUs.subtiltes_seo
            ? this.contactUs.subtiltes_seo.description
            : {},
        },
      ],
    }
  },
  nuxtI18n: {
    paths: {
      ar: `/${encodeURI('من-نحن')}`,
      en: `/about_us`,
    },
  },
}
</script>
