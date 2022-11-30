<template lang="pug">
  .search-regions

    h1.search-regions__title Регионы

    input.search-regions__input(
      v-model="region"
      @input="search"
    )

    template(v-if="addressList")
      .search-regions__list(v-for="(region, index) in addressList")
        .search-regions__item(:class={'search-regions__item_odd': index % 2 === 0 }) {{ region.name }}

      h2.search-regions__subtitle Разделы
      .search-regions__section(v-for="section in $t('sections')") {{ section }}
      .search-regions__time {{ timeLimit }}

</template>

<script>
  export default {
    name: 'ComponentSearchRegions',

    data() {
      return {
        timer: null,
        region: '',
        addressList: [],
        timeLimit: 60,
      };
    },

    methods: {
      countDown() {
        this.timeLimit--;

        if (!this.timeLimit) {
          clearInterval(this.timer);
          this.$router.push('/');
        }
      },

      /**
       * @returns {Promise<void>}
       */
      async search() {
        this.regions = await this.$api.FIAS.getAddress(this.region);
      },
    },

    mounted() {
      this.timer = setInterval(this.countDown, 1000);
    },
  }
</script>

<style lang="sass">
.search-regions
  &__title
    font-size: 1.4rem
  &__item
    &_odd
      font-size: 18px
      font-weight: bold

</style>
