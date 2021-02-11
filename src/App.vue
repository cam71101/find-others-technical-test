<template>
  <div id="app">
    <div v-bind:key="data.id" v-for="data in dataItems">
      <div v-if="!time" v-bind:data="data">
        <Button v-on:click="fetchData(data)"> {{ data }}</Button>
      </div>
    </div>

    <h2 v-if="time">{{ time }}</h2>
    <Button v-if="time" v-on:click="handleBack"> Back </Button>
    <Button v-on:click="handleHome"> Home </Button>
  </div>
</template>
<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {},
  data() {
    return {
      dataItems: [],
      areaItems: [],
      time: '',
    };
  },
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
      this.areaItems = uniqueValues;
    });
  },
  methods: {
    fetchData: function(value) {
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
    handleBack: function() {
      this.time = '';
    },
    handleHome: function() {
      this.dataItems = this.areaItems;
      this.time = '';
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
