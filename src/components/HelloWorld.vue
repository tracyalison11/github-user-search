<template>
  <div class="hello">
    <img
      src="../assets/search-icon.svg"
      alt="magnifying glass"
      height="87px"
      width="100px" />
      <input id="search-input" type="text" placeholder="looking for someone?" v-on:keyup="searchForGitHubUsers"/>
      <p v-if="error && !loaded">An error occurred during search. You may have exceeded the rate limit. Please try again.</p>
      <p v-if="loaded">Total search results: {{this.totalResults}}</p>

      <div class="search-results-table" v-if="loaded">
        <p class="mt-3">Current Page: {{ currentPage }}</p>
        <b-table
          id="my-table"
          :items="users"
          :fields="fields"
          :per-page="perPage"
          @row-clicked="rowClicked"
          :current-page="currentPage"
          small
        >
          <!-- A virtual column to show avatar -->
          <template slot="Avatar" slot-scope="data">
            <img class="avatar" :src="`${data.item.avatar_url}`" />
          </template>
        </b-table>
        <div class="overflow-auto">
          <b-pagination
            v-model="currentPage"
            :total-rows="rows"
            :per-page="perPage"
            aria-controls="my-table"
            align="fill"
          ></b-pagination>
        </div>
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
      fields: ['Avatar', {key: 'login', label: 'Username'}, {key: 'id', label: 'User ID'}, {key: 'html_url', label: 'Profile URL'}, 'starred_url', 'score'],
      totalResults: 0,
      perPage: 10,
      currentPage: 1,
      loaded: false,
      error: false,
      imageURL: ''
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
        this.users = response.data.items;
        this.totalResults = response.data.total_count;
      }, (response) => {
        this.loaded = false;
        this.error = true;
      });
    },
    rowClicked: function(row) {
      window.location.href = row.html_url;
    }
  },
  computed: {
    rows() {
      return this.users.length
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
