<template>
  <div class="profile">
    <div class="row">
      <div class="col">
        <b-icon
          icon="person-circle"
          font-scale="12"
          variant="secondary"
          class=""
        ></b-icon>
        <h2>{{username}}</h2>
      </div>
    </div>
    <b-card>
      <h4 v-if="items.length == 0">no images</h4>
      <vue-masonry-wall :items="items" :options="ImageOptions" v-else>
        <template v-slot:default="{ item }">
          <div class="Item">
            <img :src="item.image" />
          </div>
        </template>
      </vue-masonry-wall>
    </b-card>
  </div>
</template>
<script>
import VueMasonryWall from "vue-masonry-wall";
import { getUserFavorite } from "@/apis.js";
export default {
  name: "profile",
  components: { VueMasonryWall },
  data() {
    return {
      username: this.$store.state.username,
      ImageOptions: {
        width: 300,
        padding: {
          2: 8,
          default: 12,
        },
      },
      items: [],
    };
  },
  methods: {
    getImage() {
      getUserFavorite({
        userId: this.$store.state.userId,
      })
        .then((response) => {
          this.items = [];
          for (let i of response.data) {
            this.items.push({
              id: i.id,
              image: `http://localhost:3001${i.url}`,
            });
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  mounted() {
    this.getImage();
  },
};
</script>