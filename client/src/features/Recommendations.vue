<template>
  <div>
    <hr />
    <div class="container text-center">
      <div class="row display-flex">
        <!-- Loops through the array returned from hitting the API's endpoint. -->
        <div
          class="col-sm-4"
          v-for="song in recommendations.tracks"
          v-bind:key="song.id"
        >
          <div class="panel panel-default">
            <div class="panel-heading">
              <!-- Displays the song name. -->
              <h3 class="panel-title">{{ song.name }}</h3>
              <!-- Loops through a different part of the array and displays the artist's name as a link to their Spotify profile. -->
              <span
                v-for="artist in song.artists"
                v-bind:key="artist.id"
                class="insertComma"
              >
                <a v-bind:href="artist.external_urls.spotify" target="new">
                  {{ artist.name }}</a
                >
              </span>
            </div>
            <div>
              <!-- Displays the album art. -->
              <img
                class="img-responsive"
                v-bind:src="song.album.images[0].url"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { getRecommendations } from '../../utils/service'; // service.js
export default {
  name: 'recommendations',
  data() {
    return {
      // Initializes recommendations.
      recommendations: '',
    };
  },
  methods: {
    //  Calls to the getRecommendations function in service.js.
    getRecommendationsFromService() {
      // Response from service.js is stored locally.
      getRecommendations().then((recommended) => {
        this.recommendations = recommended;
      });
    },
  },
  // Executes when the page is loaded. Necessary for template access.
  mounted() {
    this.getRecommendationsFromService();
  },
};
</script>

<style scoped>
/* Fixes bootstrap grid problem */
.row.display-flex {
  display: flex;
  flex-wrap: wrap;
}
.row.display-flex > [class*='col-'] {
  display: flex;
  flex-direction: column;
}
.container.text-center {
  margin: auto;
  max-width: 600px;
}
/* Separates names with commas */
.insertComma + .insertComma:before {
  content: ', ';
  font-weight: normal;
}
h3 {
  color: black;
  font-size: 17px;
  font-weight: bold;
}
a {
  color: black;
  font-size: 17px;
  font-weight: normal;
}
</style>
