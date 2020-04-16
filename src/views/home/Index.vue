<template>
  <v-app>
    <home-core-app-bar v-model="expandOnHover" />

    <home-core-view />
  </v-app>
</template>

<script>
  export default {
    name: 'HomeIndex',

    components: {
      HomeCoreAppBar: () => import('./components/core/AppBar'),
      HomeCoreView: () => import('./components/core/View'),
    },

    data: () => ({
      expandOnHover: false,
    }),
  }
</script>
