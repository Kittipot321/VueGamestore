<template>
  <div id="page-product wrapper" class="hero-body" :style="{backgroundSize:'cover',backgroundRepeat:'no-repeat',backgroundImage:'url('+product.screenshots[0].image+')',backgroundAttachment:'fixed'}">
    <div class="box columns is-12 is-multiline section">
      <div class="column is-12">
        <div class="columns is-multiline">
          <div class="column is-2">
            <figure class="image mb-6 is-flex is-justify-content-center">
              <img :src="product.thumbnail" />
            </figure>
          </div>
          <div class="column is-6">
            <h2 class="is-size-3 has-text-left">{{ product.title }}</h2>
          </div>
          <div class="column is-4 has-text-right">
            <a :href="product.game_url" class="button is-success is-fullwidth">FREE</a>
          </div>
        </div>
        <div class="columns is-12 is-multiline">
          <div class="column is-5">
            <p class="is-size-4"><strong>About the Game</strong></p>
          </div>
          <div class="column is-7">
            <div class="columns is-12 has-text-left is-mobile">
              <div class="column is-4">
                <p><strong>Genre</strong></p>
                <p>{{ product.genre }}</p>
              </div>
              <div class="column is-4">
                <p><strong>Platform</strong></p>
                <p>{{ product.platform }}</p>
              </div>
              <div class="column is-4">
                <p><strong>Publisher</strong></p>
                <p>{{ product.publisher }}</p>
              </div>
            </div>
            <div class="columns is-12 has-text-left is-mobile">
              <div class="column is-4">
                <p><strong>Developer</strong></p>
                <p>{{ product.developer }}</p>
              </div>
              <div class="column is-4">
                <p><strong>Release Date</strong></p>
                <p>{{ product.release_date }}</p>
              </div>
            </div>
          </div>
        </div>
        <p class="has-text-left">{{ product.description }}</p><br>
        <p class="is-size-5 has-text-left"><strong>Minimum system requirements</strong></p>
        <ul>
          <li class="has-text-left">OS : {{product.minimum_system_requirements.os?product.minimum_system_requirements.os:"n/a"}}</li>
          <li class="has-text-left">CPU : {{product.minimum_system_requirements.processor?product.minimum_system_requirements.processor:"n/a"}}</li>
          <li class="has-text-left">Memory : {{product.minimum_system_requirements.memory?product.minimum_system_requirements.memory:"n/a"}}</li>
          <li class="has-text-left">Graphics : {{product.minimum_system_requirements.graphics?product.minimum_system_requirements.graphics:"n/a"}}</li>
          <li class="has-text-left">Storage : {{product.minimum_system_requirements.storage?product.minimum_system_requirements.storage:"n/a"}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
window.scrollTo(0,0)
export default {
  name: "ProductDetail",
  data() {
    return {
      product: { screenshots: [{}],minimum_system_requirements:{} }
    };
  },
  mounted() {
    this.getProduct();
    window.scrollTo(0,0)
  },
  methods: {
    async getProduct() {
      const id_slug = this.$route.params.product_id_slug;
      const options = {
        method: "GET",
        url: "https://free-to-play-games-database.p.rapidapi.com/api/game",
        params: { id: id_slug },
        headers: {
          "x-rapidapi-key":
            "f654a77917mshe591b0f35de3489p188830jsnebda0b15bf00",
          "x-rapidapi-host": "free-to-play-games-database.p.rapidapi.com",
        },
      };

      await axios
        .request(options)
        .then((response) => {
          this.product = response.data;
          document.title = this.product.title + " | Topeeshop"
        })
        .catch(function (error) {
          console.error(error);
        });
    },
  },
};
</script>

<style scoped>
img {
  width: 50%;
  height: 100%;
}
.is-horizontal-center {
  flex: 1;
  justify-content: center;
}
.box{
  margin-top: 30%
}
</style>