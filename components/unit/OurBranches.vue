<template>
  <div
    class="our-branches"
    :style="`background: linear-gradient(to top, rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.7)), url('${bgImg}');`"
  >
    <div class="container">
      <div class="row">
        <h2 class="col-12 text-center our-branches-heading">
          {{ $t('ourBranches') }}
        </h2>
        <div class="col-md-6 col-12">
          <div class="branch-item">
            <h3 class="dash-heading">{{ $t('branches.branchOne') }}</h3>
            <p class="description">{{ aboutUs.branch_one }}</p>
          </div>
        </div>
        <div class="col-md-6 col-12">
          <div class="branch-item">
            <h3 class="dash-heading">{{ $t('branches.branchTwo') }}</h3>
            <p class="description">{{ aboutUs.branch_two }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { fetchStore } from '~/mixin/fetchStore'

export default {
  name: 'OurBranches',
  props: {
    bgImg: {
      type: String,
      default: '~assets/images/bg/bg-our-branch.webp',
    },
  },
  mixins: [
    fetchStore([
      {
        stateName: 'aboutUs',
      },
    ]),
  ],
}
</script>

<style lang="scss" scoped>
.our-branches {
  color: #fff;

  /* background: linear-gradient(to top, rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.7)), */

  /*  url('~assets/images/bg/bg-our-branch.webp'); */

  background-repeat: no-repeat;
  background-size: cover;
  background-position: center center;
  padding-bottom: 3rem;

  .our-branches-heading {
    margin-top: 3rem;
  }

  .dash-heading {
    font-size: 1rem;
    color: #fff;
  }

  .description {
    line-height: 1.88;
    font-size: 0.8rem;
  }
}
</style>
