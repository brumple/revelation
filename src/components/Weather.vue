<template>
  <div class="weather-comparison">
    <div class="container">
      <div class="side-by-side">
        <h1>Weather Comparison</h1>

        <div class="legend">
          <div class="legend-item">
            <span class="legend-color orange"></span>
            <span>Greater than 5% difference</span>
          </div>
          <div class="legend-item">
            <span class="legend-color red"></span>
            <span>Greater than 10% difference</span>
          </div>
        </div>
      </div>


      <div class="side-by-side">
        <div class="location-column">
          <h2>Location 1</h2>
          <label for="location1">Search Location 1:</label>
          <input type="text" id="location1" v-model="searchTerm1" @input="searchLocations(1)" placeholder="Enter city, ZIP code, or coordinates" />
          <div v-if="searchResults1.length > 0">
            <select @change="selectLocation($event.target.value, 1)">
              <option value="">Select a location</option>
              <option v-for="result in searchResults1" :key="result.id" :value="result.id">
                {{ result.name }}, {{ result.admin1 }}, {{ result.country }}
              </option>
            </select>
          </div>
        </div>


        <div class="location-column">
          <h2>Location 2</h2>
          <label for="location2">Search Location 2:</label>
          <input type="text" id="location2" v-model="searchTerm2" @input="searchLocations(2)" placeholder="Enter city, ZIP code, or coordinates" />
          <div v-if="searchResults2.length > 0">
            <select @change="selectLocation($event.target.value, 2)">
              <option value="">Select a location</option>
              <option v-for="result in searchResults2" :key="result.id" :value="result.id">
                {{ result.name }}, {{ result.admin1 }}, {{ result.country }}
              </option>
            </select>
          </div>
        </div>
      </div>

      <div class="full-width" v-if="selectedLocation1 && selectedLocation2">
        <div class="header">
          <div class="column">
            <h3>{{ selectedLocation1.name }}, {{ selectedLocation1.admin1 }}, {{ selectedLocation1.country }}</h3>
            <p>Latitude: {{ selectedLocation1.latitude }}</p>
            <p>Longitude: {{ selectedLocation1.longitude }}</p>
          </div>
          <div class="column"><h3>Weather Forecast</h3></div>
          <div class="column">
            <h3>{{ selectedLocation2.name }}, {{ selectedLocation2.admin2 }}, {{ selectedLocation2.country }}</h3>
            <p>Latitude: {{ selectedLocation2.latitude }}</p>
            <p>Longitude: {{ selectedLocation2.longitude }}</p>
          </div>
        </div>
        <div v-if="selectedLocation1Weather && selectedLocation2Weather">
          <div v-for="(day, index) in selectedLocation1Weather" :key="index" style="margin-bottom: 50px;">
            <div class="row" style="background: aqua">
              <div class="cell">{{ day.time }}</div>
              <div class="cell">Date</div>
              <div class="cell">{{ day.time }}</div>
            </div>
            <div class="row">
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'temperature_2m_max') }">{{ day.temperature_2m_max }}°F</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'temperature_2m_max') }">Temperature Max</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'temperature_2m_max') }">{{ selectedLocation2Weather[index].temperature_2m_max }}°F</p></div>
            </div>
            <div class="row">
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'temperature_2m_min') }">{{ day.temperature_2m_min }}°F</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'temperature_2m_min') }">Temperature Min</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'temperature_2m_min') }">{{ selectedLocation2Weather[index].temperature_2m_min }}°F</p></div>
            </div>
            <div class="row">
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'sunrise') }">{{ day.sunrise }}</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'sunrise') }">Sunrise</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'sunrise') }">{{ selectedLocation2Weather[index].sunrise }}</p></div>
            </div>
            <div class="row">
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'sunset') }">{{ day.sunset }}</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'sunset') }">Sunset</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'sunset') }">{{ selectedLocation2Weather[index].sunset }}</p></div>
            </div>
            <div class="row">
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'precipitation_sum') }">{{ day.precipitation_sum }}mm</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'precipitation_sum') }">Precipitation</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'precipitation_sum') }">{{ selectedLocation2Weather[index].precipitation_sum }}mm</p></div>
            </div>
            <div class="row">
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'precipitation_probability_max') }">{{ day.precipitation_probability_max }}%</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'precipitation_probability_max') }">Precipitation Probability</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'precipitation_probability_max') }">{{ selectedLocation2Weather[index].precipitation_probability_max }}%</p></div>
            </div>
            <div class="row">
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'wind_speed_10m_max') }">{{ day.wind_speed_10m_max }}m/s</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'wind_speed_10m_max') }">Wind Max Speed</p></div>
              <div class="cell"><p :style="{ color: getDifferenceColor(index, 'wind_speed_10m_max') }">{{ selectedLocation2Weather[index].wind_speed_10m_max }}m/s</p></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>


</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      searchTerm1: '',
      searchResults1: [],
      selectedLocation1: null,
      selectedLocation1Weather: null,
      searchTerm2: '',
      searchResults2: [],
      selectedLocation2: null,
      selectedLocation2Weather: null,
    };
  },
  methods: {
    async searchLocations(locationNumber) {
      const searchTerm = locationNumber === 1 ? this.searchTerm1 : this.searchTerm2;
      try {
        const response = await axios.get(`https://geocoding-api.open-meteo.com/v1/search?name=${searchTerm}`);
        if (locationNumber === 1) {
          this.searchResults1 = response.data.results;
        } else {
          this.searchResults2 = response.data.results;
        }
      } catch (error) {
        console.error('Error searching locations:', error);
      }
    },
    selectLocation(id, locationNumber) {
      id = parseInt(id);

      if (!isNaN(id)) {
        if (locationNumber === 1) {
          this.selectedLocation1 = this.searchResults1.find(obj => obj.id === id);
          this.fetchWeather(this.selectedLocation1.latitude, this.selectedLocation1.longitude, locationNumber);
        } else {
          this.selectedLocation2 = this.searchResults2.find(obj => obj.id === id);
          this.fetchWeather(this.selectedLocation2.latitude, this.selectedLocation2.longitude, locationNumber);
        }
      }
    },
    async fetchWeather(latitude, longitude, locationNumber) {
      try {
        const response = await axios.get(`https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&daily=weather_code,temperature_2m_max,temperature_2m_min,sunrise,sunset,precipitation_sum,precipitation_probability_max,wind_speed_10m_max&temperature_unit=fahrenheit`);
        const forecast = response.data.daily;
        const truncatedForecast = forecast.time.slice(0, 5).map((day, index) => ({
          time: day,
          weather_code: forecast.weather_code[index],
          temperature_2m_max: forecast.temperature_2m_max[index],
          temperature_2m_min: forecast.temperature_2m_min[index],
          sunrise: forecast.sunrise[index],
          sunset: forecast.sunset[index],
          precipitation_sum: forecast.precipitation_sum[index],
          precipitation_probability_max: forecast.precipitation_probability_max[index],
          wind_speed_10m_max: forecast.wind_speed_10m_max[index],
        }));
        if (locationNumber === 1) {
          this.selectedLocation1Weather = truncatedForecast;
        } else {
          console.log(truncatedForecast)
          this.selectedLocation2Weather = truncatedForecast;
        }
      } catch (error) {
        console.error('Error fetching weather:', error);
      }
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      return date.toDateString();
    },
    calculateDifference(value1, value2) {
      return Math.abs((value1 - value2) / ((value1 + value2) / 2)) * 100;
    },
    getDifferenceColor(index, attribute) {
      if (this.selectedLocation1Weather && this.selectedLocation2Weather) {
        const value1 = this.selectedLocation1Weather[index][attribute]
        const value2 = this.selectedLocation2Weather[index][attribute]

        const difference = this.calculateDifference(value1, value2);

        if (difference > 10) {
          return 'red';
        } else if (difference > 5) {
          return 'orange';
        }
      }
      return 'inherit';
    },
  },
};
</script>

<style scoped>
.weather-comparison {
  display: flex;
  justify-content: space-around;
}

.container {
  display: flex;
  flex-wrap: wrap;
}

.side-by-side {
  width: 50%; /* Two divs side by side */
  box-sizing: border-box; /* Ensure padding and border are included in width */
  padding: 10px; /* Optional: Add padding */
}

.full-width {
  width: 100%;
  box-sizing: border-box;
  padding: 10px;
}

.header, .row {
  display: flex;
}

.column, .cell {
  flex: 1;
  border: 1px solid black;
  padding: 5px;
}

.location-column {
  //width: 45%;
  //margin-bottom: 20px;
}

.legend {
  clear: both;
  border: 2px solid #ccc;
  margin-top: 20px;
}

.legend-item {
  margin-bottom: 5px;
}

.legend-color {
  display: inline-block;
  width: 20px;
  height: 10px;
  margin-right: 5px;
}

.legend-color.orange {
  background-color: orange;
}

.legend-color.red {
  background-color: red;
}

li {
  list-style-type: none;
}
</style>
