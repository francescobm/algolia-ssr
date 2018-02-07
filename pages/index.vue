<template>
  <section class="container">
    <div>
      <app-logo/>
      <h1 class="title">
        Algolia SSR
      </h1>
      <h2 class="subtitle">
        Nuxt.js project
      </h2>
      <div class="links">
        <ais-index :search-store="searchStore" >
          <ais-search-box></ais-search-box>
          <ais-results>
            <template slot-scope="{ result }">
              <h2>{{ result.name }}</h2>
            </template>
          </ais-results>
        </ais-index>
      </div>
    </div>
  </section>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue'
import { createFromAlgoliaCredentials, createFromSerialized } from 'vue-instantsearch'
const searchStore = createFromAlgoliaCredentials('latency', '3d9875e51fbd20c7754e65422f7ce5e1')
searchStore.indexName = 'bestbuy'

export default {
  async asyncData () {
    searchStore.start()
    searchStore.refresh()

    await searchStore.waitUntilInSync()

    return { serializedSearchStore: searchStore.serialize() }
  },
  components: {
    AppLogo
  },
  data () {
    return {
      searchStore: null
    }
  },
  created () {
    this.searchStore = createFromSerialized(this.serializedSearchStore)
  }
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
