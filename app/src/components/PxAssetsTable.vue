<template>
  <table>
    <thead>
      <tr class="bg-gray-100 border-b-2 border-gray-400">
        <th>Logo</th>
        <th>
          <span>Ranking</span>
        </th>
        <th>Nombre</th>
        <th>Precio</th>
        <th>Cap. de Mercado</th>
        <th>Variación 24hs</th>
        <td class="hidden sm:block"></td>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="a in assets"
        :key="a.id"
        class="border-b border-gray-200 hover:bg-gray-100 hover:bg-orange-100"
      >
        <td>
          <img
            class="w-10 h-10"
            :src="`https://static.coincap.io/assets/icons/${a.symbol.toLowerCase()}@2x.png`"
            :alt="a.name"
          />
        </td>
        <td>
          <b>#{{ a.rank }}</b>
        </td>
        <td>
          <router-link
            class="hover:underline text-green-600"
            :to="{ name: 'coin-detail', params: { id: a.id } }"
          >
            {{ a.name }}
          </router-link>
          <small class="ml-1 text-gray-500"> {{ a.symbol }}</small>
        </td>
        <td>{{ dollar(a.priceUsd) }}</td>
        <td>{{ dollar(a.marketCapUsd) }}</td>
        <td
          :class="
            parseFloat(a.changePercent24Hr) < 0.0
              ? 'text-red-600'
              : 'text-green-600'
          "
        >
          {{ percent(a.changePercent24Hr) }}
        </td>
        <td class="hidden sm:block">
          <px-button @custom-click="goToCoin(a.id)">
            <span>Detalle</span>
          </px-button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import { dollarFilter } from "@/filters";
import { percentFilter } from "@/filters";
import PxButton from "@/components/PxButton";

export default {
  name: "PxAssetsTable",
  components: { PxButton },
  props: {
    assets: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    goToCoin(id) {
      this.$router.push({
        name: "coin-detail",
        params: { id },
      });
    },
    dollar(value) {
      return dollarFilter(value);
    },
    percent(value) {
      return percentFilter(value);
    },
  },
};
</script>

<style scoped>
.up::before {
  content: "👆";
}

.down::before {
  content: "👇";
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
