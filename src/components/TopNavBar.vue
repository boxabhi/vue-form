<template>

  <v-app-bar 
     color="deep-purple" dark>
    
    <v-app-bar-nav-icon @click="drawer = true"></v-app-bar-nav-icon>

    <v-toolbar-title   width="300">Title</v-toolbar-title>

    <div    class="mx-auto " style="margin-left: 1000px !important">
      <v-icon  color=" darken-2" class="m-3" style="margin: 20px;">
      mdi-palette
    </v-icon>
   
    <v-icon  color=" darken-2">
      mdi-eye
    </v-icon>
    {{formId}}
    <v-btn
    style="margin: 20px;"
  
  depressed
  class="text-dark m-3"
  
>Send</v-btn>
    </div>
  
    
  </v-app-bar>


</template>

<script>
  export default {

    props: {
    formId: String
  },
    name: 'TopNavBar',


    data: () => ({
      drawer: false,
      group: null,
    }),
  }
</script>