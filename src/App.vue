<template>
  
  <v-app>
    <v-spacer></v-spacer>
    <v-app-bar
      
      color=#FB62CE
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Kaora"
          class="shrink mr-2"
          contain
          src="https://api.kaora-partners.com/wp-content/uploads/2022/09/kaora-logo.svg"
          transition="scale-transition"
          width="130"
        />


      </div>
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
