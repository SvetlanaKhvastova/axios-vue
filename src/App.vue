<template>
  <div v-if="!loading">
    <Gallery :limit="perPage" :page="currentPage" />
    <Pagination :on-change-page="handleChangePage" :current-page="currentPage" :page-count="pageCount" />
  </div>

  <Loader v-else />
</template>

<script>
import { defineComponent, onMounted, ref, computed } from 'vue'
import Loader from './components/Loader.vue'
import Gallery from './components/Gallery.vue'
import Pagination from './components/Pagination.vue'
import { fetchAllPhotos } from './api/fetchData.js'

export default defineComponent({
  components: {
    Loader,
    Gallery,
    Pagination
  },
  setup() {
    const currentPage = ref(1)
    const perPage = ref(15)
    const photosCount = ref(0)

    const loading = ref(true)


    const handleChangePage = (page) => {
      currentPage.value = page
    }

    const pageCount = computed(() => {
      return Math.ceil(photosCount.value / perPage.value)
    })

    onMounted(() => {
      fetchAllPhotos().then((data) => {
        photosCount.value = data.length
        loading.value = false
      }).catch((error) => {
        console.error(error)
        loading.value = false
      })
    })


    return {
      currentPage,
      perPage,
      photosCount,
      handleChangePage,
      pageCount,
      loading
    }
  }
})
</script>

<style>
* {
  font-family: 'Lexend', sans-serif;
}

ul {
  list-style: none;
  padding: 0;
}
</style>
