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
<script>
import CountryVue from "./Country.vue";
export default {
  name: "CountriesVue",
  props: {
    countries: Array,
    searchInput: String,
    selectedRegion: String,
    selectedSubRegion: String,
    sortBy: String,
    mode: Boolean,
    selectedCurrency: String,
  },
  components: {
    CountryVue,
  },
  computed: {
    filterCountries() {
      let ans = this.countries;
      if (this.sortBy === "population-asc") {
        ans = this.countries.toSorted((a, b) => {
          return a.population - b.population;
        });
      }
      if (this.sortBy === "population-desc") {
        ans = this.countries.toSorted((a, b) => {
          return b.population - a.population;
        });
      }
      if (this.sortBy === "area-asc") {
        ans = this.countries.toSorted((a, b) => {
          return a.area - b.area;
        });
      }
      if (this.sortBy === "area-desc") {
        ans = this.countries.toSorted((a, b) => {
          return b.area - a.area;
        });
      }
      return ans.filter((country) => {
        if (
          (this.searchInput === "" ||
            country?.name?.common
              .toLowerCase()
              .includes(this.searchInput.toLowerCase())) &&
          (this.selectedRegion === "" ||
            this.selectedRegion === country?.region) &&
          (this.selectedSubRegion === "" ||
            this.selectedSubRegion === country?.subregion)
        ) {
          let curr =
            country?.currencies &&
            Object.values(country?.currencies).map((each) => each.name);
          console.log("70", this.selectedCurrency);
          if (
            this.selectedCurrency === "" ||
            (curr && curr.includes(this.selectedCurrency))
          ) {
            return country;
          }
        }
      });
    },
  },
};
</script>
