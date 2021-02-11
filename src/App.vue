<template>
  <div id="app">
    <div v-bind:key="data.id" v-for="data in dataItems">
      <!-- <v-button v-bind:data="data" v-on:click="fetchData" /> -->
      <div v-if="!time" v-bind:data="data">
        <Button v-on:click="fetchData(data)"> {{ data }}</Button>
      </div>
    </div>
    <h2 v-if="time">{{ time }}</h2>
  </div>
</template>
<script>
import axios from 'axios';

// import Button from './components/Button';

export default {
  // NAME APPLICATION
  name: 'App',
  // IMPORT COMPONENTS
  components: {
    // Card,
    // Spinner,
    // custom tagname so it doesn't conflict with HTML
    // 'v-header' : Header,
    // 'v-button': Button,
  },
  // STATE
  data() {
    return {
      dataItems: [],
      currentArea: '',
      time: '',
    };
  },
  //USE EFFECT
  created() {
    axios.get('http://worldtimeapi.org/api/timezone"').then((response) => {
      const array = [];
      response.data.forEach((e) => {
        if (e !== e.toUpperCase()) {
          array.push(e.split('/')[0]);
        }
      });
      let uniqueValues = [...new Set(array)];
      this.dataItems = uniqueValues;
    });
  },
  // USE MEMO
  // computed: {
  //   filteredList() {
  //     return this.streamers.filter((stream) => {
  //       return stream.name.toLowerCase().includes(this.search.toLowerCase());
  //     });
  //   },
  // },
  // FUNCTIONS GO HERE
  methods: {
    fetchData: function(value) {
      // `this` inside methods points to the Vue instance
      // `event` is the native DOM event

      if (value.includes('/')) {
        axios
          .get('http://worldtimeapi.org/api/timezone/' + value)
          .then((response) => {
            let date = new Date(response.data.unixtime * 1000);
            let hours = date.getHours();
            var minutes = '0' + date.getMinutes();
            var seconds = '0' + date.getSeconds();
            var formattedTime =
              hours + ':' + minutes.substr(-2) + ':' + seconds.substr(-2);
            response.data.time = formattedTime;
            this.time = formattedTime;
          });
      } else {
        axios
          .get('http://worldtimeapi.org/api/timezone/' + value)
          .then((response) => {
            this.dataItems = response.data;
          });
      }
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: Arial;
}
.wrapper {
  height: 80vh;
  margin-left: 20%;
  margin-right: 20%;
  margin-top: 5rem;
}
</style>
