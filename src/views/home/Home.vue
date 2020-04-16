<template>
  <v-container
    id="home"
    fluid
    tag="section"
  >
    <v-row>
      <v-col cols="12">
        <v-carousel-item>
          v-for="(item,i) in items"
          :key="i"
          :src="item.src"
        </v-carousel-item>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'HomeHome',

    data () {
      return {
        items: [
          {
            src: 'https://cdn.vuetifyjs.com/images/carousel/squirrel.jpg',
          },
          {
            src: 'https://cdn.vuetifyjs.com/images/carousel/sky.jpg',
          },
          {
            src: 'https://cdn.vuetifyjs.com/images/carousel/bird.jpg',
          },
          {
            src: 'https://cdn.vuetifyjs.com/images/carousel/planet.jpg',
          },
        ],
      }
    },
    methods: {
      complete (index) {
        this.list[index] = !this.list[index]
      },
    },

  }
</script>
