<template>
  <div class="container">
    <div class="row">
      <div class="col-lg-3 col-md-6 col-6">
        <div class="about-item text-center">
          <img src="~/assets/images/icons/about/icon-doctor.svg" alt="doctor" />

          <span class="count">{{ subTitles.doctors_number }}</span>

          <p class="item-description">{{ $t('Practicing_Physicians') }}</p>
        </div>
      </div>

      <div class="col-lg-3 col-md-6 col-6">
        <div class="about-item text-center">
          <img src="~/assets/images/icons/about/icon-clinic.svg" alt="doctor" />

          <span class="count">{{ subTitles.clinics_number }}</span>

          <p class="item-description">{{ $t('Wellness_Clinics') }}</p>
        </div>
      </div>

      <div class="col-lg-3 col-md-6 col-6">
        <div class="about-item text-center">
          <img
            src="~/assets/images/icons/about/icon-countries.svg"
            alt="doctor"
          />

          <span class="count">{{ subTitles.countries_numbers }}</span>

          <p class="item-description">{{ $t('Countries') }}</p>
        </div>
      </div>

      <div class="col-lg-3 col-md-6 col-6">
        <div class="about-item text-center">
          <img
            src="~/assets/images/icons/about/icon-employee.svg"
            alt="doctor"
          />

          <span class="count">{{ subTitles.employees_number }}</span>

          <p class="item-description">{{ $t('Employees') }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { fetchStore } from '~/mixin/fetchStore'
export default {
  name: 'AboutStatus',
  mixins: [
    fetchStore([
      {
        stateName: 'subTitles',
      },
    ]),
  ],
}
</script>
