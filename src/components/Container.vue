<template>
  <div class="containers">
    <div>
      <form @submit.prevent="searchCountry">
        <div class="inputs">
          <input
            type="text"
            placeholder="Search country....."
            v-model="country"
            autofocus
          />
          <input type="submit" value="Submit" />
        </div>
      </form>
      <div class="data-wrapper">
        <div class="country-confirmed">
          <h2>Confirmed:</h2>
          <h2>{{ confirmed }}</h2>
        </div>
        <div class="country-recovered">
          <h2>Recovered:</h2>
          <h2>{{ recovered }}</h2>
        </div>
        <div class="country-death">
          <h2>Deaths:</h2>
          <h2>{{ deaths }}</h2>
        </div>
      </div>
      <div class="updated">
        <h3>
          Updated: <em>{{ lastUpdated }}</em>
        </h3>
      </div>

      <div
        class="data-wrapper country"
        v-if="this.countryDetails.confirmed !== ''"
      >
        <div>
          <h1 class="country-name">
            {{ this.countryDetails.countryNames }}
          </h1>
        </div>
        <div class="country-confirmed">
          <h2>Confirmed:</h2>
          <h2>{{ countryDetails.confirmed }}</h2>
        </div>
        <div class="country-recovered">
          <h2>Recovered:</h2>
          <h2>{{ countryDetails.recovered }}</h2>
        </div>
        <div class="country-death">
          <h2>Deaths:</h2>
          <h2>{{ countryDetails.deaths }}</h2>
        </div>
        <h3 class="updated">
          Updated: <em>{{ countryDetails.lastUpdated }}</em>
        </h3>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Container",
  data() {
    return {
      confirmed: "",
      recovered: "",
      deaths: "",
      lastUpdated: "",
      country: "",

      countryDetails: {
        confirmed: "",
        recovered: "",
        deaths: "",
        lastUpdated: "",
        countryNames: ""
      }
    };
  },

  methods: {
    async fetchData() {
      try {
        const res = await axios.get("https://covid.mathdro.id/api");
        const data = res.data;
        this.confirmed = data.confirmed.value;
        this.recovered = data.recovered.value;
        this.deaths = data.deaths.value;
        const updateTime = new Date(data.lastUpdate);
        const realTime = updateTime.toGMTString();
        this.lastUpdated = realTime;
      } catch (error) {
        console.log("ERROR: ", error);
      }
    },
    async searchCountry() {
      try {
        const countriesRes = await axios.get(
          `https://covid19.mathdro.id/api/countries/${this.country}`
        );
        if (this.country !== "") {
          const countryData = countriesRes.data;
          this.countryDetails.confirmed = countryData.confirmed.value;
          this.countryDetails.recovered = countryData.recovered.value;
          this.countryDetails.deaths = countryData.deaths.value;
          const updateTime = new Date(countryData.lastUpdate);
          const realTime = updateTime.toGMTString();
          this.countryDetails.lastUpdated = realTime;
          const countryName = this.country;
          this.countryDetails.countryNames = countryName.toUpperCase();
          this.country = "";
        } else {
          this.$swal(`Please input a valid country`);
        }
      } catch (err) {
        this.$swal(`${this.country} is not found`);
        this.country = "";
        console.log(err);
      }
    }
  },
  mounted() {
    this.fetchData();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.containers {
  min-height: calc(100vh - 123.38px);
  padding: 1em 0 2em;
  width: 70%;
  color: white;
  margin: 0 auto;
}
.inputs {
  text-align: center;
  padding-bottom: 2em;
  display: flex;
  width: auto;
  justify-content: space-between;
}
input[type="text"] {
  border: none;
  outline: none;
  background: none;
  padding: 0.6em 1em;
  border-radius: 4px;
  width: 50%;
  box-shadow: inset -4px -4px 10px 4px #fff, inset 4px 4px 10px 4px #c4c4c4;
  color: black;
  font-size: 18px;
  font-weight: 700;
  transition: 0.2s ease-out;
}
input[type="text"]:focus {
  box-shadow: inset -1px -1px 3px 1px #fff, inset 1px 1px 3px 1px #c4c4c4;
}
input[type="submit"] {
  outline: none;
  border-radius: 4px;
  color: gray;
  cursor: pointer;
  border: 3px solid transparent;
  padding: 0.6em 1em;
  font-weight: 700;
  box-shadow: -4px -4px 13px 4px #fff, 4px 4px 13px 4px #c4c4c4;
}
input[type="submit"]:hover {
  box-shadow: -3px -3px 8px 3px #fff, 3px 3px 8px 3px #c4c4c4;
}
input[type="submit"]:focus {
  box-shadow: -4px -4px 13px 4px #fff, 4px 4px 13px 4px #c4c4c4;
}
.data-wrapper {
  width: 100%;
}
.data-wrapper div {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 1em 0.4em;
  margin: 1em 0;
  border-radius: 4px;
  box-shadow: -4px -4px 13px 4px #fff, 4px 4px 13px 4px #c4c4c4;
}
.country-name {
  color: black;
  text-align: center;
  width: 100%;
}
.country-confirmed {
  color: rgb(185, 185, 38);
}
.country-recovered {
  color: rgb(8, 141, 8);
}
.country-death {
  color: rgb(168, 6, 6);
}
.updated {
  color: black;
  padding: 0.5em 0;
}
.country {
  padding: 2em 0 1em;
}
.country div {
  margin-bottom: 1em;
}

@media screen and (max-width: 780px) {
  .containers {
    width: 95%;
  }
  input[type="text"] {
    width: 70%;
  }
  .updated em {
    font-size: 0.8em;
  }
}
</style>
