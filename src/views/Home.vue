<template>
  <div>
    <bounce-loader
      :loading="isLoading"
      :color="'#68d391'"
      :size="100"
    ></bounce-loader>
    <assets-table-component
      v-if="!isLoading"
      :assets="assets"
    ></assets-table-component>
  </div>
</template>

<script>
import AssetsTableComponent from '@/components/AssetsTableComponent'
import api from '@/api'
export default {
  name: 'Home',
  components: { AssetsTableComponent },
  data() {
    return {
      assets: [],
      isLoading: false,
    }
  },
  created() {
    this.isLoading = true
    api
      .getAssets()
      .then((assets) => {
        this.assets = assets
      })
      .finally(() => (this.isLoading = false))
  },
}
</script>

<style></style>
