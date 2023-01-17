<template>
  <v-app> 
    <v-app-bar app>  
    <router-link to="/">Home</router-link> |
    <router-link to="/SignUp">Sign Up</router-link> |
    <router-link to="/Dashboard">Dashboard</router-link>
    </v-app-bar>
    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>
