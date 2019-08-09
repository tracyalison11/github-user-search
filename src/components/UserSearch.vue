<template>
  <div class="search">
    <div class="search-box">
      <div class="search-circle" v-bind:class="{ active: isActive }">
        <img src="../assets/search-icon.svg" class="search-icon" alt="magnifying glass" @mouseover="showInput"/>
        <input class="search-input" v-bind:class="{ active: isActive }" type="text" placeholder="looking for someone?" v-on:keyup="searchForGitHubUsers"/>
        <span></span>
      </div>
    </div>

      <p class="search-results-info error" v-if="error && !loaded">An error occurred during search. You may have exceeded the rate limit. Please try again.</p>
      <p class="search-results-info" v-if="loaded">Total search results: {{Number(this.totalResults).toLocaleString()}}</p>

      <div class="search-results-table" v-if="loaded">
        <p class="search-results-info">Current Page: {{ currentPage }}</p>
        <div class="overflow-auto">
          <b-pagination
            v-model="currentPage"
            :total-rows="rows"
            :per-page="perPage"
            aria-controls="my-table"
            align="center"
          ></b-pagination>
        </div>
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
      </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'UserSearch',
  data () {
    return {
      users: [],
      fields: ['Avatar', {key: 'login', label: 'Username'}, {key: 'id', label: 'User ID'}, {key: 'html_url', label: 'Profile URL'}],
      totalResults: 0,
      perPage: 10,
      currentPage: 1,
      loaded: false,
      error: false,
      isActive: false
    }
  }
  methods: {
    searchForGitHubUsers: function() {
      var searchElement = $('.search-input');
      var searchValue = searchElement.val();

      if (searchValue) {
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
      }
    },
    rowClicked: function(row) {
      window.location.href = row.html_url;
    },
    showInput: function() {
      this.isActive = !this.isActive;
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
