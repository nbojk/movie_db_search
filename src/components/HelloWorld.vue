<template>
  <div class="hello">
    <div class="search">
      <input type="text" v-on:keyup="updateList" v-model="searchTerm" placeholder="Search...">
      <select name="type" id="type" v-model="searchType" @click="updateList">
        <option value="movie">Movie</option>
        <option value="series">Series</option>
        <option value="episode">Episode</option>
      </select>
    </div>
    <ul v-if="list">
      <li v-for="(item, index) in list" v-bind:key="index">
        <img :src="item.Poster" alt="">
        <h2> {{ item.Title }} ({{ item.Year }}) </h2>
        <p> {{ item.Type }} </p>
      </li>
    </ul>
    <p v-if="error"> {{ this.error }} </p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
  data() {
    return {
    apiKey: 'a92a67d8',
    searchTerm: '',
    error: "Search for something.",
    list: null,
    searchType: ''
    }
  },
  methods: {
    updateList: function() {
      axios.get('http://www.omdbapi.com/?s=' + this.searchTerm  + '&type=' + this.searchType +  '&apikey=a92a67d8')
      .then((response) => {
        if(response.data.Response == "True") {
          this.list = response.data.Search;
          this.error = null;
        } else {
          if(response.data.Error == "Something went wrong." ) {
            this.error = "Search for something."
          } else {
          this.error = response.data.Error;
          }
          this.list = null;
          
        }
      })
      .catch(this.error = "Something went wrong, please try again later.")
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .hello {
    width: 800px;
    margin: 0 auto;
    overflow: hidden;
    .search {
      width: 100%;
      height: 80px;
      background-color: #1cbc9b;
      border-radius: 10px;
      #type {
        float: right;
        border-radius: 10px;
      }
      input {
        vertical-align: center;
        background-color: #1cbc9b;
        border: none;
        color: #2d3e50;
        width: 600px;
        height: 75px;
        font-size: 25px;
        border-radius: 10px;
        float: left;
        font-weight: 700;
        &:focus {
          outline: none;
        }
      }
    }
    ul {
    list-style-type: none;
    width: 100%;
    li {
      background-color: #2d3e50;
      margin-right: 75px;
      border-radius: 10px;
      p {
        color: #fff;
      }
      h2 {
        color: #fff;
      }
    }
    } 
    ::placeholder {
      color: #2d3e50;
    }
  }
</style>
