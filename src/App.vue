<template>
  <div class="wrapper">
    <div v-bind:key="data.id" v-for="data in dataItems">
      <div v-if="!time" v-bind:data="data">
        <Button v-on:click="fetchData(data)" class="button-location">
          {{ data }}</Button
        >
      </div>
    </div>

    <h2 v-if="time">{{ time }}</h2>
    <div class="btn-wrapper">
      <Button v-if="time" v-on:click="handleBack" class="button"> Back </Button>
      <Button v-on:click="handleHome" class="button"> Home </Button>
    </div>
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
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.btn-wrapper {
  display: flex;
}
.button {
  background-color: red;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 1rem;
}
button:hover {
  color: var(--color-hover);
  cursor: pointer;
}

.button-location {
  background-color: #4caf50;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 1rem;
}

button-location:hover {
  color: var(--color-hover);
  cursor: pointer;
}
</style>
