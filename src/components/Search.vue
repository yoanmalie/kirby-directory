<template>
  <section class="search">

    <input class="search__searchfield" name="" type="text" v-model="search" @keyup="searchQuery"
      placeholder="search all plugins"
      />

    <button class="search__searchbutton" @click="searchQuery">
      <font-awesome-icon icon="search" color="#333" />
    </button>

  </section>
</template>

<script>
import { mapState, mapActions, mapGetters } from 'vuex'
import debounce from 'tiny-debounce'

export default {
  name: 'Search',
  computed: {
    search: {
      get () {
        return this.$store.getters.getSearchTerm
      },
      set (value) {
        // this.$store.commit('SET_SEARCH_QUERY', value)
        this.setSearchQuery(value)
      }
    },
    ...mapState([
      'items',
      'displayedItems'
    ]),
    ...mapGetters([
      'getLoading'
    ])
  },
  mounted () {
  },
  methods: {
    ...mapActions([
      'getResultsFilter',
      'getResultsSearch',
      'setSearchQuery'
    ]),
    searchQuery: debounce(function () {
      if (this.search !== '') {
        this.$router.push({name: 'Search', params: {query: this.$store.getters.getSearchTerm, page: 1}})
        // this.getResultsSearch({query: this.search, page: 1})
      } else {
        this.$router.push({name: 'ListStart', params: {page: 1}})
      }
    }, 500)
  },
  beforeRouteLeave: function (to, from, next) {
    if (from.name === 'Search') {
      this.$store.dispatch('removeQuery')
      this.search = ''
    }
    next()
  }
}
</script>

<style lang="scss">
@import './../assets/scss/_vars.scss';

.search {
  position: relative;
  top: 1rem;
  width: 100%;
  margin-top: .5rem;
  margin-bottom: 6rem;

  @media screen and (min-width: $xs) {
    margin-bottom: 6rem;
  }

  &__searchfield {
    position: relative;
    width: 100%;
    padding: 1rem;
    background: white;
    box-shadow: 0;
    appearance: none;
    border: 1px solid $cBorder;
  }

  &__searchbutton {
    position: absolute;
    top: 1rem;
    right: 1rem;
    appearance: none;
    -webkit-appearance: none;
    border: 0;
    background: 0;
    img {
        width: 2rem;
        height: 2rem;
    }
  }
}
</style>
