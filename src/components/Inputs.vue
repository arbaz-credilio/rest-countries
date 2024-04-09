<template>
  <input
    type="text"
    v-model="searchInput"
    @input="$emit('search-countries', searchInput)"
    :class="mode && 'dark-input'"
  />
  <select
    v-model="selectedRegion"
    @change="$emit('region-filter', selectedRegion)"
  >
    <option value="">Select region</option>
    <option v-for="region in regions" :key="region" :value="region">
      {{ region }}
    </option>
  </select>
  <select
    name="subregion"
    id="subregion"
    v-model="selectedSubRegion"
    @change="$emit('subregion-filter', selectedSubRegion)"
  >
    <option value="">Select subregion</option>
    <option v-for="subregion in subregions" :value="subregion" :key="subregion">
      {{ subregion }}
    </option>
  </select>
  <select
    name="sort"
    id="sort"
    v-model="sortBy"
    @change="$emit('sort-by', sortBy)"
  >
    <option value="">sort by</option>
    <option value="population-asc">ascending by population</option>
    <option value="population-desc">descending by population</option>
    <option value="area-asc">ascending by area</option>
    <option value="area-desc">descending by area</option>
  </select>
  <select
    name="currencies"
    id="currencies"
    v-model="selectedCurrency"
    @change="$emit('seletced-currency', selectedCurrency)"
  >
    <option value="">select currencies</option>
    <option v-for="currency in currencies" :key="currency" :value="currency">
      {{ currency }}
    </option>
  </select>
</template>
<script>
export default {
  name: "InputsVue",
  data() {
    return {
      searchInput: "",
      selectedRegion: "",
      selectedSubRegion: "",
      sortBy: "",
      selectedCurrency: "",
    };
  },
  props: {
    regions: Array,
    subregions: Array,
    mode: Boolean,
    currencies: Array,
  },
  emits: [
    "search-countries",
    "region-filter",
    "subregion-filter",
    "sort-by",
    "seletced-currency",
  ],
};
</script>
