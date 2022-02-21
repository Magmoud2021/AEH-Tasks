<template>
  <div class="contact-us">
    <transition name="slide-fade">
      <div v-if="showMessage" class="success-message">
        {{ formSuccessMessage }}
      </div>

      <div v-if="showErrorMessage" class="faild-message">
        {{ formErrorMessage }}
      </div>
    </transition>

    <SectionHeadline :title="$t('contactUs')" />

    <div class="container pb-5 pt-0 pt-lg-5">
      <div class="row">
        <div class="col-md-4 col-12 contact-info">
          <div class="row">
            <div class="col-2 text-center">
              <i class="far fa-map" :style="`color: ${accentColor}`"></i>
            </div>

            <div class="col-10">
              <p class="header-info">{{ $t('location') }} :</p>

              <p>
                <a
                  class="sub-info"
                  target="_blank"
                  href="https://www.google.com/maps/d/embed?mid=1lH6tundP8-eLtThBoYay22lHS-mfi5GX&hl=en"
                  >{{ contactUs.address }}</a
                >
              </p>

              <p>
                <a
                  class="sub-info"
                  target="_blank"
                  href="https://www.google.com/maps/d/embed?mid=1lH6tundP8-eLtThBoYay22lHS-mfi5GX&hl=en"
                  >{{ contactUs.address_two }}</a
                >
              </p>
            </div>

            <div class="col-2 text-center">
              <i class="fas fa-phone-alt" :style="`color: ${accentColor}`"></i>
            </div>

            <div class="col-10">
              <p class="header-info">{{ $t('telephone') }} :</p>

              <a class="sub-info phone-ar" :href="`tel:${contactUs.phone}`">{{
                contactUs.phone
              }}</a>
            </div>

            <div class="col-2 text-center">
              <i class="fas fa-envelope" :style="`color: ${accentColor}`"></i>
            </div>

            <div class="col-10">
              <p class="header-info">{{ $t('emailAddress') }} :</p>

              <a class="sub-info" :href="`mailto:${contactUs.email}`">{{
                contactUs.email
              }}</a>
            </div>
          </div>
        </div>

        <div class="col-md-8 col-12">
          <client-only>
            <ValidationObserver
              v-slot="{ invalid }"
              ref="contactForm"
              class="d-block"
            >
              <div class="row">
                <div class="col-12">
                  <p v-if="showFormDescription">{{ formDescription }}</p>

                  <form @submit.prevent="onSubmit">
                    <div class="row">
                      <div class="col-md-6 col-12">
                        <ValidationProvider
                          v-slot="{ errors }"
                          name="Name"
                          rules="required"
                          class="general-input"
                        >
                          <input
                            v-model="contactInfo.name"
                            type="text"
                            :placeholder="$t('contact.name')"
                          />
                          <span class="error-message">{{ errors[0] }}</span>
                        </ValidationProvider>
                      </div>

                      <div class="col-md-6 col-12">
                        <ValidationProvider
                          v-slot="{ errors }"
                          name="phone"
                          :rules="{ required, regex: /^01\d{9}$/, max: 11 }"
                          class="general-input"
                        >
                          <input
                            v-model="contactInfo.phone"
                            type="number"
                            :placeholder="$t('contact.phone')"
                          />
                          <span class="error-message">{{ errors[0] }}</span>
                        </ValidationProvider>
                      </div>

                      <div class="col-12">
                        <ValidationProvider
                          v-slot="{ errors }"
                          name="Email"
                          rules="required|email"
                          class="general-input"
                        >
                          <input
                            v-model="contactInfo.email"
                            type="email"
                            :placeholder="$t('contact.email')"
                          />
                          <span class="error-message">{{ errors[0] }}</span>
                        </ValidationProvider>
                      </div>

                      <div class="col-12 text-area-div">
                        <ValidationProvider
                          v-slot="{ errors }"
                          name="Message"
                          rules="required"
                          class="contact-text-area"
                        >
                          <textarea
                            v-model="contactInfo.message"
                            :placeholder="$t('contact.message')"
                          ></textarea>
                          <span class="error-message">{{ errors[0] }}</span>
                        </ValidationProvider>
                      </div>

                      <div class="col-12">
                        <button
                          type="submit"
                          :disabled="invalid"
                          class="send-btn send-btn-home"
                          :style="`backgroundColor: ${accentColor}`"
                        >
                          {{ $t('sendMessage') }}
                        </button>
                      </div>
                    </div>
                  </form>
                </div>
              </div>
            </ValidationObserver>
          </client-only>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { fetchStore } from '~/mixin/fetchStore'
import { contactUsObj } from '~/mixin/contactUsMixin'
import SectionHeadline from '~/components/unit/SectionHeadline'
export default {
  name: 'ContactUsForm',
  components: { SectionHeadline },
  mixins: [
    fetchStore([
      {
        stateName: 'contactUs',
      },
    ]),
    contactUsObj,
  ],
  props: {
    langBtn: {
      type: String,
      required: true,
    },
    formSuccessMessage: {
      type: String,
      required: true,
    },
    formErrorMessage: {
      type: String,
      required: true,
    },
    accentColor: {
      type: String,
      required: true,
    },
    contactApi: {
      type: String,
      required: true,
    },
    showFormDescription: {
      type: Boolean,
      required: false,
      default: true,
    },
    formDescription: {
      type: String,
      required: false,
      default: 'This is the default vaule for formDescription!',
    },
  },
  data() {
    return {
      required: null,
    }
  },

}
</script>

<style scoped lang="scss">
.contact-us {
  background: #f7f7f7;
}

.send-btn-home {
  margin: 0 auto 0 0;
  border-radius: 0.3rem;
  padding: 0.5rem 2rem;
  color: #fff;
  height: 45px;
  font-size: 1rem;
  opacity: 1;

  &:disabled {
    opacity: 0.85;
  }
}

.contact-info {
  background: #fff;
  border-radius: 0.3rem;
  max-height: 300px;
  display: flex;
  justify-content: center;
  align-items: center;

  .header-info {
    color: #5d5d5d;
  }

  p {
    margin-bottom: 0;
  }

  @media (max-width: 992px) {
    background: transparent;
    display: block;
    font-size: 0.8rem;
    margin-bottom: 1rem;
  }
}

.fa-map,
.fa-phone-alt,
.fa-envelope {
  font-size: 1.8rem;
  margin-top: 1.2rem;
}
</style>
