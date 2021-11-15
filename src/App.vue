<template>
  <div
    id="wheater-app"
    :class="
      typeof wheater.main != 'undefined' && wheater.main.temp > 16 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          placeholder="Search..."
          class="search-input"
          v-model="query"
          @keypress="fetchWheater"
        />
      </div>
      <!-- error city not found -->
      <h1 class="error" v-if="err != ''">{{ err }}</h1>
      <!-- ------------------ -->
      <div class="wheater-wrap" v-if="typeof wheater.main != 'undefined'">
        <div class="location-box">
          <h3 class="location">
            {{ wheater.name }}, {{ wheater.sys.country }}
          </h3>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="wheater-box">
          <div class="temp">{{ Math.round(wheater.main.temp) }}°c</div>
        </div>
        <div class="wheater"></div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      apiKey: "b245f9f735c068055616c3d73b12d554",
      urlBase: "https://api.openweathermap.org/data/2.5/",
      wheater: {},
      query: "",
      currentAdress: "",
      err: "",
    };
  },
  created() {
    this.getWheater("tangier");
  },
  methods: {
    getWheater(q) {
      fetch(`${this.urlBase}weather?q=${q}&units=metric&APPID=${this.apiKey}`)
        .then((res) => {
          if (res.status == 404) {
            this.err = "city not found";
          }
          return res.json();
        })
        .then(this.setResults);
    },
    fetchWheater(e) {
      this.err = "";
      if (e.key == "Enter") {
        this.getWheater(this.query);
      }
    },
    setResults(results) {
      this.wheater = results;
    },
    dateBuilder() {
      let date = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday",
      ];

      return `${months[date.getMonth()]} - ${
        days[date.getDay()]
      }- ${date.getFullYear()}`;
    },
  },
};
</script>


<style>
body {
  font-family: "Cairo", sans-serif;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#wheater-app {
  margin: 0 auto;
  background: url("./assets/cold-bg.jpg") no-repeat bottom;
  background-size: cover;
  height: 100vh;
}
#wheater-app .error {
  text-align: center;
  color: #fff;
  background-color: rgb(230, 36, 65);
  width: 350px;
  margin: 50px auto;
}
#wheater-app.warm {
  background: url("./assets/warm-bg.jpg") no-repeat bottom;
  background-size: cover;
}
#wheater-app .search-box {
  padding: 15px;
}
#wheater-app .search-box input {
  border: none;
  outline: none;
  padding: 15px;
  color: #222;
  background: #fff;
  display: block;
  border-radius: 5px;
  width: 100%;
  transition: ease 0.4s;
  font-size: 18px;
}
#wheater-app .search-box input::placeholder {
  color: inherit;
  font-weight: 500;
}

#wheater-app .search-box input:focus {
  box-shadow: 1px 1px 8px rgba(100, 100, 100, 0.5);
}

.wheater-wrap {
  text-align: center;
}

.wheater-wrap .location-box {
  margin-top: 15px;
}
.wheater-wrap .location-box .location {
  font-size: 45px;
  color: #fff;
  text-shadow: 5px 5px 6px rgba(9, 9, 9, 0.8);
  letter-spacing: 2px;
  margin-bottom: 0;
}
.wheater-wrap .location-box .date {
  color: #fff;
  font-style: italic;
  font-size: 15px;
  font-weight: bold;
  margin-top: -15px;
  text-shadow: 5px 1px 6px rgba(9, 9, 9, 0.8);
}

.wheater-box {
  text-align: center;
}

.wheater-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 100px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 10px;
  margin: 30px 0;
  box-shadow: 3px 2px 12px 1px rgba(0, 0, 0, 0.5);
}
</style>
