<template>
  <section>
    <div class="container">
      <h2 v-if="enteredUsername">The gists of user {{ enteredUsername }}:</h2>
      <h3 v-if="noContentFound || usersGists.length === 0">No content found</h3>
      <gist-item
        v-for="gist in usersGists"
        :key="gist.id"
        :id="gist.id"
        :files="gist.files"
      ></gist-item>
    </div>
  </section>
</template>

<script>
import GistItem from "./GistItem.vue";
export default {
  props: {
    enteredUsername: String,
  },
  data() {
    return {
      usersGists: [],
      noContentFound: false,
    };
  },
  watch: {
    enteredUsername(newUsername) {
      this.noContentFound = false;
      this.usersGists = [];

      const axios = require("axios");

      axios
        .get("https://api.github.com/users/" + newUsername + "/gists")
        .then((response) => {
          if (response.data) {
            this.usersGists = response.data;
          }
        })
        .catch((error) => {
          console.log(
            "The following error occurred while trying to fetch the gists of the user ",
            newUsername,
            error
          );
          this.noContentFound = true;
        });
    },
  },
  components: {
    GistItem,
  },
};
</script>

<style scoped></style>>
