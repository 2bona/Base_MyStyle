<template>
  <div class="d-flex align-center justify-space-between">
    <p @click="$router.push('/')" class="font-weight-black mb-0 grey--text" style="font-size: 25px"><span class="black--text">My</span>Style</p>
    <v-btn outlined color="grey darken-2" v-if="token" to="/myshop" depressed> my designs </v-btn>
    <v-avatar @click="$router.push('/profile')" size="60" class="">
      <v-img :src="img2"></v-img>
    </v-avatar>
  </div>
</template>
<script>
// @ is an alias to /src

export default {
  name: "headernav",
 
  data: () => ({
      img2:'https://res.cloudinary.com/base-uni/image/upload/v1658076722/alpha_connect/C7E03945-30FB-49E5-90AF-FE6D67322900_kegufu.png'
  }),
     computed:{
    token(){
      return this.$store.getters.getToken
    },
     }
};
</script>
