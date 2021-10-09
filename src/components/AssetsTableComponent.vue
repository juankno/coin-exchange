<template>
  <table class="table-auto w-full">
    <thead>
      <tr class="bg-gray-100 border-b-2 border-gray-400">
        <th></th>
        <th :class="{ up: this.sortOrder == 1, down: this.sortOrder == -1 }">
          <span class="underline cursor-pointer" @click="shangeSortOrder"
            >Ranking</span
          >
        </th>
        <th>Nombre</th>
        <th>Precio</th>
        <th>Cap. de Mercado</th>
        <th>Variación 24hs</th>
        <td class="hidden sm:block">
          <input
            class="
              bg-gray-100
              focus:outline-none
              border-b border-gray-400
              py-2
              px-4
              block
              w-full
              appearance-none
              leading-normal
            "
            id="filter"
            placeholder="Buscar..."
            type="search"
            v-model="filter"
          />
        </td>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="asset in filteredAseets"
        :key="asset.id"
        class="border-b border-gray-200 hover:bg-gray-100"
      >
        <td>
          <img
            class="w-6 h-6"
            :src="`https://static.coincap.io/assets/icons/${asset.symbol.toLowerCase()}@2x.png`"
            :alt="asset.name"
          />
        </td>
        <td>
          <b># {{ asset.rank }}</b>
        </td>
        <td>
          <router-link
            class="hover:underline text-green-600"
            :to="{ name: 'coin-detail', params: { id: asset.id } }"
          >
            {{ asset.name }}
          </router-link>
          <small class="ml-1 text-gray-500">
            {{ asset.symbol }}
          </small>
        </td>
        <td>{{ asset.priceUsd | dollar }}</td>
        <td>{{ asset.marketCapUsd | dollar }}</td>
        <td
          :class="
            asset.changePercent24Hr
              ? asset.changePercent24Hr.includes(`-`)
                ? `text-red-600`
                : `text-green-600`
              : ''
          "
        >
          {{ asset.changePercent24Hr | percent }}
        </td>
        <td class="hidden sm:block">
          <button-component @navigate-button="goToCoin(asset.id)">
            <span>Detalle</span>
          </button-component>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import ButtonComponent from '@/components/ButtonComponent'

export default {
  name: 'AssetsTableComponent',
  components: { ButtonComponent },
  data() {
    return {
      filter: '',
      sortOrder: 1,
    }
  },
  props: {
    assets: {
      type: Array,
      default: () => [],
    },
  },

  computed: {
    filteredAseets() {
      const altOrder = this.sortOrder == 1 ? -1 : 1
      return this.assets
        .filter(
          (a) =>
            a.symbol.toLowerCase().includes(this.filter.toLowerCase()) ||
            a.name.toLowerCase().includes(this.filter.toLowerCase())
        )
        .sort((a, b) => {
          if (parseInt(a.rank) > parseInt(b.rank)) {
            return this.sortOrder
          }
          return altOrder
        })
    },
  },

  methods: {
    goToCoin(id) {
      this.$router.push({ name: 'coin-detail', params: { id } })
    },
    shangeSortOrder() {
      this.sortOrder = this.sortOrder == 1 ? -1 : 1
    },
  },
}
</script>

<style scoped>
.up::before {
  content: '👆';
}

.down::before {
  content: '👇';
}

td {
  padding: 20px 0px;
  font-size: 0.6rem;
  text-align: center;
}

th {
  padding: 5px;
  font-size: 0.6rem;
}

@media (min-width: 640px) {
  td,
  th {
    padding: 20px;
    font-size: 1rem;
  }

  th {
    padding: 12px;
  }
}
</style>
