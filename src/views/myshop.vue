<template>
  <div class="mycourses">
    <v-container style="max-width:990px">
           <headernav />
      <v-layout wrap class="pt-12 d-flex align-start">
<v-flex xs12 md8 class=" ">
  <v-flex xs12>

<p class=" grey--text " style="font-size:20px">Add New Design</p>

<v-form  onSubmit="return false;" class="mb-6" style="max-width:630px" >
         <v-select
          v-model="category"
          :items="categories"
          label="Select your product's category"
          required
        ></v-select>
        <v-text-field
          v-model="name"
          :counter="11"
          label="Product Name"
          required
        ></v-text-field>
        <v-text-field
          v-model="price"
          label="Price"
          required
        ></v-text-field>
          <v-file-input
    label="Product Image"
     :loading="loading2"
    @change="fileChange"
    prepend-icon="mdi-camera"
  ></v-file-input>
        <v-textarea
          v-model="description"
          label="Description"
          required
        ></v-textarea>
   
  
   

   
      <v-btn
        class="font-weight-bold mr-4" color="#903813"
        type="submit" large dark
        :loading="loading || loading2"
        @click="addProduct()"
      >
        submit
      </v-btn>
     </v-form>
  </v-flex>

      <v-layout wrap>

<p class=" grey--text " style="font-size:20px">My Designs</p>
      <v-flex xs12>
        <v-layout wrap>

       <v-flex class="pa-3" v-for="(n, i) in my_products" :key="i" xs6 sm4>
        <v-card>
          <v-avatar tile height="200px" width="100%"><v-img contain :src="n.image|| 'https://res.cloudinary.com/base-uni/image/upload/v1658076722/alpha_connect/C7E03945-30FB-49E5-90AF-FE6D67322900_kegufu.png'"></v-img></v-avatar>
          <div class="pa-2">

<p class="mb-2 font-weight-medium">{{n.name}}</p>
<p class="mb-2">N{{n.description | description}}</p>
<v-btn @click="deleteProduct(n.id)" :loading="loading" color="red" dark icon class="font-weight-bold" small><v-icon>mdi-trash-can</v-icon></v-btn>
          </div>
        </v-card>
      </v-flex>
              </v-layout>
  <p v-if="!my_products.length && !loading">No Lessons Yet</p>
      </v-flex>
      </v-layout>
</v-flex>
<v-flex :class="$vuetify.breakpoint.smAndUp? 'pl-4': ''" xs12 md4>
<adcard /></v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import axios from "axios";
const headernav = () => import("../components/headernav.vue");
const adcard = () => import("../components/adcard.vue");
import imageCompression from 'browser-image-compression';

// @ is an alias to /src

export default {
  name: 'mycourses',
   components: {
    headernav,
    adcard,
    },
  data: () => ({
    loading:false,
    category: '',
    name: '',
    price: '',
    productImage: '',
    loading2:false,
    attachments: [],
    description: '',
    categories: ['Casual wears', 'Ankara', 'Gown', 'Shirts', 'Trousers', 'Accessories'],
    img: "https://res.cloudinary.com/base-uni/image/upload/v1658076824/alpha_connect/GWC_ALt_logo_1_zkglzr.png"
  }),
     computed:{
    product(){
      return this.$store.getters.getProduct
    },
    user(){
      return this.$store.getters.getUser
    },
        my_products: {
      get() {
        return this.$store.getters.getMyProducts;
      },
      set(val) {
        this.$store.dispatch("setMyProducts", val);
      }
    },
     },
     mounted(){
       this.getMyProducts()
     },
  methods:{
      fileChange(e) {
        if(!e.name)return
          
          this.attachments = [];
      
            this.loading2 = true;
console.log(e)
 
        var options = {
    maxSizeMB: 1,
    maxWidthOrHeight: 1920,
    useWebWorker: true
  }

  imageCompression(e, options)
    .then( (compressedFile)=> {
      this.attachments.push(compressedFile);
      this.loading2 = false;
    })
    .catch(function () {
      this.loading2 = false;
    });
    },
    deleteProduct(x){
      this.loading = true
      axios.get('product/delete?id='+x).then(()=>{
        this.getMyProducts()
      alert('Product has been deleted')
      this.loading = false
      })
    },

    addProduct(){
      const sn = this
      this.loading = true
      const config = { headers: { "Content-Type": "multipart/form-data" } };
    const fd = new FormData();
   
        fd.append("files[" + 0 + "]", sn.attachments[0]);
        fd.append("name", sn.name)
        fd.append("price", sn.price)
        fd.append("description", sn.description)
        fd.append("user_id", sn.user.id)
        fd.append("category", sn.category)
        fd.append("shop", 'mystyle')
    
      axios.post('product/create',fd, config).then((res)=>{
        this.my_products = res.data.success
      this.loading = false
       })
    },
    
    getMyProducts(){
      this.loading = true
      axios.get('product/my_products').then((res)=>{
        this.my_products = res.data.products
      this.loading = false
       })
    }
  }
}
</script>
