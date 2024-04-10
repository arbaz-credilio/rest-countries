<template>
  <navbar-vue @color-mode="(param) => (mode = param)" />
  <div class="inputs">
    <input
      type="text"
      v-model="searchInput"
      @input="$emit('search-countries', searchInput)"
      :class="mode && 'dark-input'"
    />
    <filter-vue
      :filterName="'Region'"
      :vModelValue="selectedRegion"
      :emit="'region-filter'"
      :emitValue="selectedRegion"
      :options="regions"
      @region-filter="
        (param) => {
          selectedRegion = param;
          selectedSubRegion = '';
          handleSubregion(selectedRegion);
        }
      "
    />
    <filter-vue
      :filterName="'Sub Region'"
      :emit="'subregion-filter'"
      :vModelValue="selectedSubRegion"
      :options="subregion"
      @subregion-filter="
        (param) => {
          selectedSubRegion = param;
        }
      "
    />
    <filter-vue
      :filterName="'sort by'"
      :emit="'sort-by'"
      :options="sorts"
      :vModelValue="sortBy"
      @sort-by="(param) => (sortBy = param)"
    />
    <filter-vue
      :filterName="'select currency'"
      :emit="'seletced-currency'"
      :options="currencies"
      :vModelValue="selectedCurrency"
      @seletced-currency="(param) => (selectedCurrency = param)"
    />
  </div>
  <div class="countries" :class="mode && 'dark-countries'">
    <Countries
      :mode="mode"
      :countries="countries"
      :searchInput="searchInput"
      :selectedRegion="selectedRegion"
      :selectedSubRegion="selectedSubRegion"
      :sortBy="sortBy"
      :selectedCurrency="selectedCurrency"
    />
  </div>
</template>

<script setup>
import Countries from "./components/CountriesVue.vue";
import axios from "axios";
import NavbarVue from "./components/NavbarVue.vue";
import FilterVue from "./components/FilterVue.vue";
import { onMounted, ref } from "vue";
let mode = ref(false);
let countries = ref([]);
let searchInput = ref("");
let selectedRegion = ref("");
let regions = ["Asia", "Africa", "Americas", "Europe", "Oceania", "Antarctic"];
let subregion = ref([]);
let selectedSubRegion = ref("");
let sorts = ref(["population-asc", "population-desc", "area-asc", "area-desc"]);
let sortBy = ref("");
let currencies = ref([]);
let selectedCurrency = ref("");
const handleSubregion = (region) => {
  subregion.value = [];
  countries.value.forEach((country) => {
    if (country?.region === region) {
      if (!subregion.value.includes(country?.subregion)) {
        subregion.value.push(country?.subregion);
      }
    }
  });
};
const getAllCountries = async () => {
  const response = await axios.get("https://restcountries.com/v3.1/all");
  countries.value = response.data;
  currencies = [
    ...new Set(
      countries.value.map((country) => {
        return (
          country?.currencies &&
          country?.currencies !== "" &&
          Object.values(country?.currencies)[0]?.name
        );
      })
    ),
  ];
};
onMounted(() => getAllCountries());

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.countries {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}
.country {
  width: 350px;
}
.dark-input {
  background: black;
  color: white;
}
.dark-country {
  background: rgb(54, 54, 54);
  color: white;
}
.dark-countries {
  background: black;
}
.dark-navbar {
  background: black;
}
</style>
