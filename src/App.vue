<template>
  <TheHeader />
  <NavigationBar />
  <LinksPath />
  <FavoriteOffers :offers="offers"/>
  <TheFooter />
</template>

<script>
import TheHeader from './components/header/the-header.vue'
import NavigationBar from './components/nav-bar/navigation-bar.vue';
import LinksPath from './components/links/links-path.vue'
import FavoriteOffers from './components/favorite-offers/favorite-offers.vue';
import TheFooter from './components/footer/the-footer.vue';

import OffersData from './db/db.json'

export default {
  name: 'App',
  components: {
    TheHeader,
    NavigationBar,
    LinksPath,
    FavoriteOffers,
    TheFooter,
  },
  data() {
    return {
      offers: OffersData
    }
  }
}
</script>

<style>
* {
  margin: 0;
  font-family: 'Red Hat Text', sans-serif;
  font-size: large;
}
</style>
