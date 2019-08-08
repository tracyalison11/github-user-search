<template>
  <div class="hello">
    <!-- <h1>{{ msg }}</h1> -->

    <img
      src="../assets/search-icon.svg"
      alt="magnifying glass"
      height="87px"
      width="100px" />
      <input id="search-input" type="text" placeholder="looking for someone?" v-on:keyup="searchForGitHubUsers"/>
      <p v-if="loaded">Total search results: {{this.totalResults}}</p>
      <div v-for="user in users">
        {{user}}
      </div>

    </div>

</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
  data () {
    return {
      users: [],
      totalResults: 0,
      loaded: false
    }
  },
  props: {
    msg: String
  },
  methods: {
    searchForGitHubUsers: function() {
      var searchElement = $('#search-input');
      var searchValue = searchElement.val();
      searchValue = searchValue.toString();

      axios.get('https://api.github.com/search/users', {
        params: {
          q: searchValue
        }
      }).then((response) => {
        this.loaded = true;
        console.log(response);
        this.users = response.data.items;
        this.totalResults = response.data.total_count;
      })
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
