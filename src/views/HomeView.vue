<template>
  <div class="home">
    <div class="header">
      <my-input v-model='searchText' @input='debounce(() => {
        this.fetchCountries()
      }, 300)'>
        <div class='hints'>
          <span class='hint' v-for='hint in searchHints' :key='hint' @click='() => {
            this.searchText = hint
            this.fetchWeather()
            this.searchHints = []
          }'>{{ hint }}</span>
        </div>
      </my-input>
      <my-button @click='fetchWeather'>Search</my-button>
    </div>

    <div class='content'>
      <div v-for='location in   locations' :key='location.id' class='card'>
        <h2 class='heading'>{{ location.city }}</h2>
        <div class='weather'>
          <img :src='location.icon' />
          <p>{{ location.weather }} <strong>{{ location.temp }}°C</strong></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';

export default {
  name: 'HomeView',
  data() {
    return {
      locations: [],
      searchText: '',
      searchHints: []
    }
  },
  methods: {
    async fetchWeather() {
      try {
        const { data } = await axios.get('http://api.weatherapi.com/v1/forecast.json', {
          params: {
            key: 'c25fa9cc62a14fa89f290642232301',
            q: this.searchText
          }

        })
        const location = {
          id: data.location.localtime_epoch,
          city: data.location.name,
          region: data.location.region,
          country: data.location.country,
          weather: data.current.condition.text,
          icon: data.current.condition.icon,
          temp: data.current.temp_c,
        }
        this.locations.push(location)
        this.searchText = ''
      } catch (error) {
        console.log(error?.message)
      }
    },
    async fetchCountries() {
      try {
        const { data } = await axios.get(`https://restcountries.com/v2/name/${this.searchText}`)
        console.log(data)
        this.searchHints = data.slice(0, 3).map(d => d.name)
      } catch (error) {
        console.log(error?.message)
      }
    }
  },
  setup() {
    function createDebounce() {
      let timeout = null;
      return function (fnc, delayMs) {
        clearTimeout(timeout);
        timeout = setTimeout(() => {
          fnc()
        }, delayMs)
      }
    }

    return {
      debounce: createDebounce(),
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
  display: flex;
  flex-direction: column;
}

.header {
  display: flex;
  flex-direction: row;
  column-gap: 20px;
  margin: 0 auto;
}

.content {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
  column-gap: 20px;
  row-gap: 10px;
  flex-wrap: wrap;

}

.hints {
  display: flex;
  flex-direction: column;


  .hint {
    border: 2px solid rgb(19, 140, 196);
    margin-top: 5px;
    padding: 10px 20px;
    border-radius: 5px;
    width: 250px;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;

  }
}

.card {
  width: 250px;
  height: 220px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  border: 2px solid rgb(19, 140, 196);
  border-radius: 15px;
  padding: 5px 20px;
  column-gap: 20px;

}

.heading {
  font-size: 24px;
  margin-bottom: auto;
}


.weather {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  column-gap: 40px;
  font-size: 22px;
  font-weight: 500;
}
</style>
