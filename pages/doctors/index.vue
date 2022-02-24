<template>
  <div class="hold-doctors">
    <TopPhoto :bg-img="Setting.doctors_header_image" />

    <BreadCrumb
      current-page-link="Doctors"
      :current-page-translate="$t('menu.doctors')"
    />

    <div class="container">
      <h1 class="dash-heading">{{ $t("menu.doctors") }}</h1>

      <div v-if="allDoctors.length" class="row">
        <div
          v-for="(doctor, index) in allDoctors"
          :key="index"
          class="col-lg-4 col-md-6 col-12"
        >
          <DoctorUnit :doctor="doctor" />
        </div>
      </div>

      <div v-else class="row">
        <h4 class="text-center">{{ $t("no_found") }}</h4>
      </div>

      <v-pagination
        v-if="all && totalItemsLength > 9"
        v-model="currentPage"
        :page-count="Math.ceil(totalItemsLength / 9)"
        class="mt-3 mb-3 d-flex justify-content-center"
        :classes="bootstrapPaginationClasses"
        @change="onChangePaging"
      ></v-pagination>
    </div>
  </div>
</template>

<script>
import { mapState, mapGetters } from "vuex";
import vPagination from "vue-plain-pagination";
// import { updateSeo } from '~/mixin/updateSeo'

export default {
  name: "DoctorsPage",
  components: {
    vPagination,
  },
  nuxtI18n: {
    paths: {
      ar: `/${encodeURI("doctors")}`,
      en: `/doctors`,
    },
  },
  async asyncData({ app, $axios }) {
    const allDoctors = await $axios.get(
      `api/all_doctors?lang=${app.i18n.locale}&page=1&limit=9`
    );
    const clinics = await $axios.get(`/api/clinics?lang=${app.i18n.locale}`);
    const specialities = await $axios.get(
      `/api/specialities?lang=${app.i18n.locale}`
    );

    return {
      allDoctors: allDoctors.data.data,
      totalItemsLength: allDoctors.data.count,
      clinics: clinics.data.data,
      specialities: specialities.data.data,
    };
  },
  data() {
    return {
      rotated: false,
      all: true,
      currentPage: 1,
      branch_id: "",
      clinic_id: "",
      speciality_id: "",
      clinics: [],
      specialities: [],
      bootstrapPaginationClasses: {
        ul: "pagination",
        li: "page-item",
        liActive: "active",
        liDisable: "disabled",
        button: "page-link",
      },
    };
  },


  computed: {
    ...mapGetters(["Setting"]),
    ...mapState("contactUs", ["en_contactUs", "ar_contactUs"]),
    ...mapState("branches", ["en_branches", "ar_branches"]),
    contactUs() {
      return this.$i18n.locale === "en" ? this.en_contactUs : this.ar_contactUs;
    },
    branches() {
      return this.$i18n.locale === "en" ? this.en_branches : this.ar_branches;
    },
    seoObj() {
      return this.contactUs?.doctor_seo;
    },
  },
  mounted() {
    if (!this.$store.state.branches.en_loaded) {
      this.$store.dispatch("branches/load_en_loaded");
      this.$store.dispatch("branches/load_en_branches");
    }

    if (!this.$store.state.branches.ar_loaded) {
      this.$store.dispatch("branches/load_ar_loaded");
      this.$store.dispatch("branches/load_ar_branches");
    }
  },
  methods: {
    getDoctorsInit(page) {
      this.$axios
        .get(
          `/api/all_doctors?branch_id=${this.branch_id}&clinic_id=${this.clinic_id}&speciality_id=${this.speciality_id}&lang=${this.$i18n.locale}&page=${page}&limit=9`
        )
        .then((res) => {
          this.allDoctors = res.data.data;
        })
        .catch((error) => console.log(error.data));
    },
    onChangePaging() {
      this.getDoctorsInit(this.currentPage);
    },
    filter() {
      this.$axios
        .get(
          `/api/all_doctors?branch_id=${this.branch_id}&clinic_id=${this.clinic_id}&speciality_id=${this.speciality_id}&page=1&lang=${this.$i18n.locale}`
        )
        .then((res) => {
          this.allDoctors = res.data.data;
          this.totalItemsLength = res.data.count;
        });
    },
    async filterChange() {
      try {
        if (!this.branch_id) {
          this.branch_id = "";
        }

        this.resetInputs();

        await this.fetchClinics();
        await this.fetchSpecialities();

        this.filter();
      } catch (error) {}
    },
    async clinicChange() {
      try {
        this.resetSpecialitiesInputs();

        const specialities = await this.$axios.$get(
          `/api/specialities?lang=${this.$i18n.locale}&branch_id=${this.branch_id}&clinic_id=${this.clinic_id}`
        );

        const specialitiesData = await specialities?.data;

        this.specialities = specialitiesData;

        this.filter();
      } catch (error) {}
    },
    specialitiesChange() {
      try {
        this.filter();
      } catch (error) {}
    },
    resetInputs() {
      this.clinic_id = "";
      this.speciality_id = "";
      this.clinics = [];
      this.specialities = [];
    },
    resetSpecialitiesInputs() {
      this.speciality_id = "";
      this.specialities = [];
    },
    async fetchClinics() {
      const clinics = await this.$axios.$get(
        `/api/clinics?lang=${this.$i18n.locale}&branch_id=${this.branch_id}`
      );

      const clinicsData = await clinics?.data;

      this.clinics = clinicsData;
    },
    async fetchSpecialities() {
      const specialities = await this.$axios.$get(
        `/api/specialities?lang=${this.$i18n.locale}&branch_id=${this.branch_id}&clinic_id=${this.clinic_id}`
      );

      const specialitiesData = await specialities?.data;

      this.specialities = specialitiesData;
    },
  },
};
</script>

<style scoped lang="scss">
@import "~assets/scss/var.scss";
.booking {
  margin-top: -3rem;
  position: relative;
  z-index: 2;

  .sub-heading-booking {
    @media (max-width: 1080px) {
      font-size: 0.8rem;
    }
  }
}

.disabled {
  pointer-events: none;
}

.rotateClass {
  animation: rotate ease-in-out 1s infinite;
}

@keyframes rotate {
  0% {
    transform: rotate(0);
  }

  100% {
    transform: rotate(-360deg);
  }
}

.load-more-btn {
  display: block;
  background: transparent;
  border: none;
  margin: 1.5rem auto;
  color: #1474be;

  span {
    display: block;
  }
}

::v-deep {
  .page-link {
    border: none !important;
    color: #000 !important;
  }
  .page-item.active .page-link {
    background-color: map-get($colors, accent);
    border-color: map-get($colors, accent);
    color: #fff !important;
  }
}
.booking__btn {
  display: block;
  width: 100%;
  button {
    display: block;
    width: 100%;
  }
}
</style>
