<template>
  <div class="doctor-detail">


    <TopPhoto :bg-img="Setting.doctors_header_image" />

    <BreadCrumb
      :current-page-link="$t('menu.doctors')"
      :current-page-translate="$t('menu.doctors')"
      :sub-page="doctor.title"
      :is-sub="true"
    />

    <div class="container">
      <div class="row">
        <div class="col-lg-9 col-12">
          <div class="single-doctor-detail">
            <div class="row">
              <div class="col-lg-3 col-12 padding-right">
                <div class="hold-doctor-img">
                  <img
                    :src="doctor.photo"
                    :alt="doctor.image_alt"
                    :title="doctor.image_title"
                    class="img-fluid"
                  />
                </div>
              </div>

              <div class="col-lg-9 col-12 padding-left">
                <div class="for-border">
                  <h3 class="doctor-name">
                    <span class="doctor-specialty-icon"
                      ><img
                        src="@/assets/images/icons/icon-doctor-detail.png"
                        class="img-fluid"
                        alt="specialty icon" /></span
                    >{{ doctor.title }}
                  </h3>

                  <span class="doctor-specialty">{{ doctor.speciality }}</span>

                  <p class="doctor-address">{{ doctor.address }}</p>
                </div>
              </div>
            </div>
          </div>

          <div class="about-edu-doctor-detail">
            <!-- eslint-disable vue/no-v-html -->
            <div v-html="doctor.description"></div>
          </div>
        </div>

        <div class="col-lg-3 col-12"></div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapGetters } from 'vuex'
import TopPhoto from '~/components/unit/TopPhoto'
import BreadCrumb from '~/components/unit/BreadCrumb'
export default {
  name: 'DoctorPage',
  components: {  TopPhoto,BreadCrumb  },
  nuxtI18n: {
    paths: {
      ar: `/${encodeURI('doctor')}/:slug`,
      en: `/doctor/:slug`,
    },
  },
  filters: {
    cutParagraph(value) {
      if (!value) return
      if (value.length > 600) {
        return value.slice(0, 425) + '...'
      } else {
        return value
      }
    },
  },
  layout: 'contactUsLayout',
  /* eslint-disable */
  validate({ app, params, store }) {
    store.dispatch('doctor/reset_en_doctor')
    store.dispatch('doctor/reset_ar_doctor')

    const thePromise = new Promise(async (resolve, reject) => {
      if (app.i18n.locale === 'en') {
        if (!store.state.doctor.en_loaded) {
          await store.dispatch('doctor/load_en_doctor', params.slug)
        }

        resolve(store.state.doctor.en_doctor)
      } else {
        if (!store.state.doctor.ar_loaded) {
          await store.dispatch('doctor/load_ar_doctor', params.slug)
        }

        resolve(store.state.doctor.ar_doctor)
      }
    })

    return thePromise.then((data) => {
      return data.slug === params.slug
    })
  },
  async fetch() {
    if (!this.$store.state.doctor.en_loaded) {
      await this.$store.dispatch(
        'doctor/load_en_doctor',
        this.$route.params.slug
      )
    }

    if (!this.$store.state.doctor.ar_loaded) {
      await this.$store.dispatch(
        'doctor/load_ar_doctor',
        this.$route.params.slug
      )
    }

    await this.$store.dispatch('i18n/setRouteParams', {
      ar: { slug: this.ar_doctor.slug },
      en: { slug: this.en_doctor.slug },
    })
  },
  head() {
    return {
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.doctor.meta_description,
        },
      ],
    }
  },
  computed: {
    ...mapGetters(['Setting']),
    ...mapState('doctor', ['en_doctor', 'ar_doctor']),
    doctor() {
      return this.$i18n.locale === 'en' ? this.en_doctor : this.ar_doctor
    },
  },
}
</script>

<style scoped lang="scss">
@import '~assets/scss/var.scss';

.single-doctor-detail {
  box-shadow: 0 0 6px 0 rgba(0, 0, 0, 0.16);
  border: solid 1px map-get($colors, accent);
  border-radius: 0.2rem;
  color: #737373;
  position: relative;
  margin-bottom: 2rem;
  padding: 1rem 0;

  .hold-doctor-img {
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;

    img {
      width: 140px;
      height: 140px;
      border-radius: 50%;
      box-shadow: 0 3px 6px 0 rgba(0, 0, 0, 0.16);
      border: solid 1px rgba(183, 183, 183, 0.5);
    }

    @media (max-width: 992px) {
      margin: 1rem 0;
    }
  }

  .doctor-name {
    color: #5d5d5d;
    display: flex;
    align-items: center;
    font-weight: 600;
    margin-top: 1rem;

    .doctor-specialty-icon {
      display: inline-flex;
      justify-content: center;
      align-items: center;
      margin-right: 0.5rem;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background: #f2f2f2;

      img {
        object-fit: contain;
        width: 26px;
        height: 26px;
        display: block;
      }
    }

    @media (max-width: 992px) {
      font-size: 1.2rem;
    }
  }

  .doctor-specialty {
    color: #f39e25;
    font-weight: 600;
  }

  .doctor-address {
    font-weight: 600;
    font-size: 0.8rem;
    margin: 0.5rem auto;
  }

  .doctor-description {
    width: 95%;
  }

  .btn-doctor-detail {
    position: absolute;
    top: 8%;
    right: 5%;
    margin: 0;
    padding: 0.3rem 2.3rem;

    @media (max-width: 992px) {
      display: inline-block;
      position: relative;
      top: auto;
      right: auto;
    }
  }

  .for-border {
    border-left: 1px solid #dcdcdc;
    padding-left: 2rem;

    @media (max-width: 992px) {
      border-left: none;
      padding-left: 1rem;
    }
  }

  .padding-left {
    padding-left: 0;

    @media (max-width: 992px) {
      padding-left: 15px;
    }
  }

  .padding-right {
    padding-right: 0;

    @media (max-width: 992px) {
      padding-right: 15px;
    }
  }
}

.about-edu-doctor-detail {
  background: url('~assets/images/bg/bg-aboutus-shape.png') no-repeat center;
  background-size: contain;
}

html:lang(ar) {
  .single-doctor-detail {
    .doctor-name {
      text-align: right;

      .doctor-specialty-icon {
        margin-right: 0;
        margin-left: 0.5rem;
      }
    }

    .btn-doctor-detail {
      left: 5%;
      right: auto;

      @media (max-width: 992px) {
        left: auto;
      }
    }

    .for-border {
      border-left: none;
      border-right: 1px solid #dcdcdc;
      padding-left: 0;
      padding-right: 2rem;

      @media (max-width: 992px) {
        border-right: none;
        padding-right: 1rem;
      }
    }

    .padding-left {
      padding-right: 0;

      @media (max-width: 992px) {
        padding-right: 15px;
      }
    }

    .padding-right {
      padding-left: 0;

      @media (max-width: 992px) {
        padding-left: 15px;
      }
    }
  }

  .about-edu-doctor-detail {
    .about-section {
      border-left: none;
      border-right: 1px solid #93a19b;
      margin-right: auto;
      padding-left: 0;
      padding-right: 2rem;
    }
  }
}
</style>
