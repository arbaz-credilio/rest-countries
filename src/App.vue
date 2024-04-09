<template>
  <navbar-vue @color-mode="(param) => (mode = param)" />
  <div class="inputs">
    <inputs-vue
      @search-countries="(input) => (searchInput = input)"
      :regions="regions"
      :subregions="subregion"
      :mode="mode"
      @sort-by="(param) => (sortBy = param)"
      @region-filter="
        (param) => {
          selectedRegion = param;
          selectedSubRegion = '';
          handleSubregion(selectedRegion);
        }
      "
      @subregion-filter="(param) => (selectedSubRegion = param)"
      :currencies="currencies"
      @seletced-currency="(param) => (selectedCurrency = param)"
    />
  </div>
  <div class="countries" :class="mode && 'dark-countries'">
    <countries-vue
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

<script>
import CountriesVue from "./components/Countries.vue";
import InputsVue from "./components/Inputs.vue";
import axios from "axios";
import NavbarVue from "./components/Navbar.vue";
export default {
  name: "App",
  data() {
    return {
      mode: false,
      countries: [],
      searchInput: "",
      selectedRegion: "",
      regions: ["Asia", "Africa", "Americas", "Europe", "Oceania", "Antarctic"],
      subregion: [],
      selectedSubRegion: "",
      sortBy: "",
      currencies: [],
      selectedCurrency: "",
    };
  },
  components: {
    NavbarVue,
    CountriesVue,
    InputsVue,
  },
  methods: {
    async getAllCountries() {
      const response = await axios.get("https://restcountries.com/v3.1/all");
      this.countries = response.data;
      this.currencies = [
        ...new Set(
          this.countries.map((country) => {
            return (
              country?.currencies &&
              country?.currencies !== "" &&
              Object.values(country?.currencies)[0]?.name
            );
          })
        ),
      ];
    },
    handleSubregion(region) {
      this.subregion = [];
      this.countries.forEach((country) => {
        if (country?.region === region) {
          if (!this.subregion.includes(country?.subregion)) {
            this.subregion.push(country?.subregion);
          }
        }
      });
    },
  },
  mounted() {
    this.getAllCountries();
  },
};
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
