<template>
  <div class="home">
    <form>
      <button tupe="submit" class="button" @click.prevent="getImages">New Image</button>
      <button tupe="submit" class="button" @click.prevent="prevImage">Previous</button>
    </form>
     <p v-if="!images.length">Please Click the New Image Button to begin</p>
     <img v-if="images.length" :src="images" class="current-image"/>
  </div>
</template>

<script>
import axios from 'axios';
import apiKey from '../config';

export default {
  name: 'home',
  data() {
    return {
      loading: false,
      images: [],
      lastImage: [],
    };
  },
  methods: {
    getImages: async function () {
      this.lastImage = this.images;
      const result = await this.fetchImages();
      this.images = result.data.baseimageurl;
    },
    fetchImages() {
      this.loading = true;
      const randomId = () => Math.floor(Math.random() * (80000 - 24000) + 24000);
      return axios({
        method: 'GET',
        url: `https://api.harvardartmuseums.org/image/${randomId()}`,
        params: {
          apikey: apiKey,
        },
      });
    },
    prevImage() {
      this.images = this.lastImage;
    },
  },
};
</script>
<style lang="css">

h1 {
  margin: none;
}
.button {
  padding: .5rem 1rem;
  font-size: 1rem;
  border-radius: 20px;
  background: black;
  border: none;
  color: whitesmoke;
  margin-right: 10px;
  margin-left: 10px;
  width: 200px;
  height: 40px;
}
.button:hover {
  background: whitesmoke;
  color: black;
  border: 2px solid black;
}
.current-image {
  height: 75vh;
  width: 70vw;
  margin-top: 15px;
  border-radius: 20px;
}
</style>
