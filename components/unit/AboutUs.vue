<template>
  <div class="about-us">
    <AboutStatus v-if="String($route.name).match(/about/i) === null" />
    <div
      class="container"
      :style="
        String($route.name).match(/about/i) !== null ? 'paddingTop: 90px' : ''
      "
    >
      <div class="row">
        <div class="col-lg-6 col-12">
          <div v-if="showFrame" class="about-video">
            <div style="overflow: hidden; position: relative; width: 100%">
              <iframe
                width="560"
                height="315"
                :src="aboutUs.home_about_us_video + '?autoplay=1'"
                frameborder="0"
                allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen
                sandbox="allow-scripts allow-same-origin allow-popups"
              ></iframe>
            </div>
          </div>

          <div v-else class="video-wrapper">
            <i
              class="fas fa-play video-wrapper__icon"
              @click="showFrameFunc"
            ></i>

            <img
              class="video-wrapper__img"
              :src="aboutUs.image"
              alt="YouTube Thumbnails"
            />
          </div>
        </div>

        <div class="col-lg-6 col-12">
          <div class="about-us-content">
            <h1 class="about-us-header">{{ $t("about_us") }}</h1>

            <p
              v-if="String($route.name).match(/about/i) === null"
              class="about-us-description"
            >
              {{ aboutUs.about_us }}
            </p>

            <p v-else class="about-us-description">
              {{ aboutUs.about_us }}
            </p>

            <nuxt-link
              v-if="String($route.name).match(/about/i) === null"
              :to="
                $i18n.locale === 'en'
                  ? localePath('/about_us')
                  : localePath(`/${encodeURI('من-نحن')}`)
              "
              class="btn-main"
              >{{ $t("readMore") }}</nuxt-link
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { fetchStore } from "~/mixin/fetchStore";

export default {
  name: "AboutUs",
  filters: {
    cutParagraph(value) {
      if (!value) return;
      if (value.length > 600) {
        return value.slice(0, 425) + "...";
      } else {
        return value;
      }
    },
  },
  mixins: [
    fetchStore([
      {
        stateName: "subTitles",
      },
      {
        stateName: "aboutUs",
      },
    ]),
  ],
  data() {
    return {
      showFrame: false,
    };
  },
  methods: {
    showFrameFunc() {
      this.showFrame = true;
    },
  },
};
</script>

<style scoped lang="scss">
.about-us {
  background: url(~assets/images/bg/bg-aboutus-shape.png) fixed no-repeat center
    bottom;
  background-size: contain;
  padding-bottom: 60px;
}

.about-item {
  margin: 3rem 0;

  img {
    display: block;
    width: 35px;
    height: 47px;
    margin: 0 auto;
    object-fit: contain;
  }

  .count {
    display: block;
    color: #347b40;
    font-size: 1.2rem;
    font-weight: bolder;
  }

  .item-description {
    color: #3d505d;
    font-weight: 600;
    letter-spacing: 0.21px;
    margin: 0;
  }
}

.about-us-header {
  font-size: 2rem;
  font-weight: bold;
  text-transform: capitalize;
  color: #5d5d5d;
}

.about-us-description {
  color: #737373;
  line-height: 1.87;
  font-size: 1.2rem;
  height: 320px;
  overflow: hidden;
}

.about-us-content {
  position: relative;
  width: 92%;
  margin-left: auto;
  height: 100%;

  .btn-main {
    position: relative;

    &:hover {
      background-color: #347b40;
      color: #fff;
    }

    @media (max-width: 1080px) {
      position: relative;
      text-align: center;
    }
  }

  @media (max-width: 1080px) {
    width: 100%;
    margin-top: 2rem;
  }
}

.video-wrapper {
  overflow: hidden;
  position: relative;
  width: 100%;

  &__icon {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 64px;
    color: #fff;
    cursor: pointer;
  }

  &__img {
    width: 100%;
  }
}
</style>
