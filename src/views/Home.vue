<template>
  <v-container style="max-width:990px" class="home">
  <headernav />

<v-layout  class="pt-5" wrap>
  <v-flex class="my-8" xs12>
<v-card outlined color="#f5f5f5" min-height="50vh">
  <v-layout class="  d-flex justify-space-between align-center" wrap>

  <v-flex :class="$vuetify.breakpoint.smAndUp? '': 'text-center'" md6 xs12>

  <p class="pl-0  font-weight-medium" style="font-size:50px;">Your Fashion Is<br> Our Passion.</p>
  <v-btn v-if="!token" @click="$router.push('/signup')" x-large style="color:" depressed color="#903813" dark  class="font-weight-black px-12 text-capitalize my-4">Get Started</v-btn>
  </v-flex>
  <v-flex md6 style="position:relative" xs12>
    <div style="position:absolute; z-index:99;height:100%;
    left: 0px;
    width: 60%;
    background: linear-gradient(to right, #f5f5f5 6%, rgba(0, 0, 0, 0) 100%);"></div>
    <v-avatar tile height="50vh"  width="100%">

  <v-img style="background-position:top center!important" src="https://res.cloudinary.com/payhospi/image/upload/v1659083424/ankara_eprfhi.jpg"></v-img>
    </v-avatar>
  </v-flex>
  </v-layout>
</v-card>
  </v-flex>
  <v-flex xs12 md8>
    <div style="overflow-x:scroll" class="py-3 mb-6">
<div style="" class="d-inline-flex pt-0">
<v-chip :dark="category== ''" :color="category== ''? '#903813':'#f5f5f5'" @click="getProducts()" style="width:auto" class="rounded-sm mr-3">All</v-chip>
<v-chip :dark="category== n"  :color="category== n? '#903813':'#f5f5f5'" @click="getProductsByCategory(n)" style="width:auto" class="rounded-sm mr-3" v-for="(n, i) in categories" :key="i">{{n}}</v-chip>
<div class="px-5"></div>
</div>
</div>
    <v-layout wrap>
      <v-flex v-show="loading" class="pa-3" v-for="(n) in ['j', 'k', 'l']" :key="n" xs6 sm4>
        <v-card outlined color="grey lighten-3" width="100%" height="308px" >
        </v-card>
      </v-flex>
      <v-flex v-show="!loading" class="pa-2 mb-4" v-for="(n, i) in products" :key="i" xs6 sm4>
        <v-card outlined @click="openProduct(n)">
          <v-avatar tile height="200px" width="100%"><v-img  :src="n.image || 'https://res.cloudinary.com/base-uni/image/upload/v1658076722/alpha_connect/C7E03945-30FB-49E5-90AF-FE6D67322900_kegufu.png'"></v-img></v-avatar>
          <div class="pa-2">

<p class="mb-2 text-capitalize text-truncate font-weight-medium">{{n.name}}</p>
<p style="font-size:14px" class="grey--text text--darken-2 mb-3">{{n.description | description}}</p>
<v-btn color="#903813" dark class="font-weight-bold" small>enquire</v-btn>
          </div>
        </v-card>
      </v-flex>
      <p v-if="!loading && !products.length">No items in this category yet</p>
    </v-layout>
  </v-flex>
<v-flex :class="$vuetify.breakpoint.smAndUp? 'pl-4': ''" xs12 md4>
<adcard />
</v-flex></v-layout>
  </v-container>
</template>

<style>

.v-image__image.v-image__image--cover {
    background-position: top center!important;
}

</style>

<script>
// @ is an alias to /src
import axios from "axios";

const headernav = () => import("../components/headernav.vue");
const adcard = () => import("../components/adcard.vue");

export default {
  name: 'Home',
  components: {
    headernav,
     adcard
    },
      data: () => ({
    loading:false,
   category: '',
    categories: ['Casual wears', 'Ankara', 'Gown', 'Shirts', 'Trousers', 'Accessories'],
    description: '',
    img2: "https://res.cloudinary.com/base-uni/image/upload/v1658076722/alpha_connect/C7E03945-30FB-49E5-90AF-FE6D67322900_kegufu.png",
  }),
  computed:{
    token(){
      return this.$store.getters.getToken
    },
    user(){
      return this.$store.getters.getUser
    },
    product: {
      get() {
        return this.$store.getters.getProduct;
      },
      set(val) {
        this.$store.dispatch("setProduct", val);
      }
    },
    products: {
      get() {
        return this.$store.getters.getProducts;
      },
      set(val) {
        this.$store.dispatch("setProducts", val);
      }
    },
  },
    mounted(){
this.getProducts()
  },
  methods:{
      getProducts(){
        this.loading = true
        this.category = ''
        axios.get('/product?shop=mystyle').then((res)=>{
          this.products = res.data.products
          this.loading = false
  })
      },
      getProductsByCategory(x){
        this.loading = true
        this.category = x
        axios.get('/product/category_products?category='+x).then((res)=>{
          this.products = res.data.products
          this.loading = false
  })
      },
    openProduct(x){
      this.product = x
      this.$router.push('/product')
    }
  }
}
</script>
