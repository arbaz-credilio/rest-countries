<template>
  <div class="country-image">
    <img :src="countryImage" alt="" />
  </div>
  <div class="country-content">
    <h2>{{ countryName }}</h2>
    <p>
      <span>Population: </span><span>{{ population }}</span>
    </p>
    <p>
      <span>Region: </span><span>{{ region }}</span>
    </p>
    <p>
      <span>Capital: </span><span v-if="capital">{{ capital[0] }}</span>
    </p>
    <p v-for="currency in currencies" :key="currency">{{currency}}</p>
  </div>
</template>
<script>
export default {
  name: "CountryVue",
  props: {
    countryImage: String,
    countryName: String,
    capital: Array,
    region: String,
    population: Number,
    mode: Boolean,
    currencies: Array
  },
};
</script>
