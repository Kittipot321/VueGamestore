<template>
  <div class="home">
    <section class="section">
      <div class="columns is-multiline">
        <div class="column is-12">
          <h2 class="is-size-3 has-text-left">All Games</h2>
          <input class="input" type="text" v-model.trim="search" placeholder="Search" id="Myinput" @keyup="getLatestProducts">
          <hr/>
        </div>
      </div>
        <div class="columns is-multiline">
          <div class="column is-3" v-for="p in latestProducts" :key="p.id">
            <div class="box">
              <figure class="image mb-4">
                <img :src="p.thumbnail">
              </figure>
              <div class="box-content">
                <h3 class="is-size-4">{{p.title}}</h3>
                <p class="is-size-6 has-text-grey">{{p.short_description}}</p>
              </div>
              <div class="box-footer">
                <router-link :to='"/"+p.id' class="button is-primary">View Details</router-link>
              </div>
            </div>
          </div>
        </div>
    </section>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import axios from "axios";
import { ref } from "vue";
export default {
  name: "Home",
  components: {
    HelloWorld,
  },
  data() {
    return {
      latestProducts: [],
      search: "",
    };
  },
  created() {
    this.getLatestProducts();
  },
  mounted(){
    document.title = "Discover | topeeshop"
  },
  methods: {
    async getLatestProducts() {
      const options = {
        method: "GET",
        url: "https://free-to-play-games-database.p.rapidapi.com/api/games",
        headers: {
          "x-rapidapi-key":
            "f654a77917mshe591b0f35de3489p188830jsnebda0b15bf00",
          "x-rapidapi-host": "free-to-play-games-database.p.rapidapi.com",
        },
      };
      this.$store.commit('setIsLoading',true)
      await axios
        .request(options)
        .then((response) => {
          if (this.search) {
            this.latestProducts = response.data.filter((i) => {
              return i.title.toLowerCase().includes(this.search.toLowerCase());
            });
          } else {
            this.latestProducts = response.data;
          }
        })
        .catch(function (error) {
          console.error(error);
        });
        this.$store.commit('setIsLoading',false)
    },
  },
};
</script>

<style scoped>
.box {
  display: flex;
  flex-direction: column;
  height: 100%;
}
.box .box-content {
  flex: auto;
}
</style>
