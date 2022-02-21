<template>
  <div class="contactus-layout">
    <WhatsMessenger />
    <Header />
    <Nuxt />

    <ContactUsForm
      :lang-btn="'#lang__btn'"
      :form-success-message="$t('sentsuccesfully')"
      :form-error-message="$t('sentFaild')"
      :accent-color="'#1474be'"
      :contact-api="'/api/contacts'"
      :show-form-description="false"
    />

    <Footer />
  </div>
</template>

<script>
export default {
  name: 'ContactUsLayout',
  head() {
    return {
      htmlAttrs: {
        lang: this.$i18n.locale,
        dir: this.$i18n.locale === 'ar' ? 'rtl' : 'ltr',
      },
      link: [
        {
          rel: 'canonical',
          href: this.$config.BASE_URL + this.$route.path,
        },
      ],
    }
  },
}
</script>
