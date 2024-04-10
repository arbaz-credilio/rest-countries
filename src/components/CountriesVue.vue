<template>
  <div
    class="country"
    :class="mode && 'dark-country'"
    v-for="country in filterCountries"
    :key="country.cca3"
  >
    <country-vue
      :countryImage="country?.flags?.png"
      :countryName="country?.name?.common"
      :population="country?.population"
      :capital="country?.capital"
      :region="country?.region"
      :currencies="
        country?.currencies &&
        Object.values(country?.currencies).map((currency) => currency.name)
      "
    />
  </div>
</template>
<script setup>
import CountryVue from "./CountryVue.vue";
import { computed, defineProps } from "vue";
const props = defineProps({
  countries: Array,
  searchInput: String,
  selectedRegion: String,
  selectedSubRegion: String,
  sortBy: String,
  mode: Boolean,
  selectedCurrency: String,
});
const filterCountries = computed(() => {
  let ans = props.countries;
  if (props.sortBy === "population-asc") {
    ans = props.countries.toSorted((a, b) => {
      return a.population - b.population;
    });
  }
  if (props.sortBy === "population-desc") {
    ans = props.countries.toSorted((a, b) => {
      return b.population - a.population;
    });
  }
  if (props.sortBy === "area-asc") {
    ans = props.countries.toSorted((a, b) => {
      return a.area - b.area;
    });
  }
  if (props.sortBy === "area-desc") {
    ans = props.countries.toSorted((a, b) => {
      return b.area - a.area;
    });
  }
  return ans.filter((country) => {
    if (
      (props.searchInput === "" ||
        country?.name?.common
          .toLowerCase()
          .includes(props.searchInput.toLowerCase())) &&
      (props.selectedRegion === "" ||
        props.selectedRegion === country?.region) &&
      (props.selectedSubRegion === "" ||
        props.selectedSubRegion === country?.subregion)
    ) {
      let curr =
        country?.currencies &&
        Object.values(country?.currencies).map((each) => each.name);
      if (
        props.selectedCurrency === "" ||
        (curr && curr.includes(props.selectedCurrency))
      ) {
        return country;
      }
    }
  });
});
</script>
