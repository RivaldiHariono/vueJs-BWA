<template>
  <div class="product">
    <headerAldi />
        <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more text-left">
                        <a href="./home.html"><i class="fa fa-home"></i> Home</a>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-4">
                            
                            <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                                <div class="product-pic-zoom">
                                    <img class="product-big-img" :src="default_image"/>
                                </div>
                                <carousel class="product-thumbs-track ps-slider" 
                                :loop=false :autoplay=true :nav=false>
                                    <div class="pt" 
                                    @click="changeImage(productimg.photo)" 
                                    :class="productimg.photo == is_default ? 'active' : '' " 
                                    v-for="productimg in productDetails.galleries" 
                                    :key="productimg.id">
                                        <img v-bind:src="productimg.photo" alt="" />
                                    </div>
                                </carousel>
                            </div>

                            <div v-else>
                                <img alt="No Image" />
                            </div>

                        </div>
                        <div class="col-lg-8">
                            <div class="product-details text-left">
                                <div class="pd-title">
                                    <span>{{productDetails.type}}</span>
                                    <h3>{{productDetails.name}}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p v-html="productDetails.description">
                                    </p>
                                    <h4>Rp{{productDetails.price}}</h4>
                                </div>
                                <div class="quantity">
                                    <router-link to="/shoppingcart">
                                    <a @click="saveKeranjang(productDetails.id,productDetails.name,productDetails.price,productDetails.galleries[0].photo)" href="#" class="primary-btn pd-cart">Add to Cart</a>
                                    </router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->
    <relatedAldi />
    <footerAldi />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

import headerAldi from '@/components/headerAldi.vue';
import footerAldi from '@/components/footerAldi.vue';
import relatedAldi from '@/components/relatedAldi.vue';
import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
  name: 'product',
  components: {
    headerAldi,
    footerAldi,
    carousel,
    relatedAldi
  },
  data(){
      return{
          default_image: "",
          images: [],
          productDetails: [],
          keranjangUser: []
      }
  },
  methods: {
    //   methods bisa lebih dari 1
      changeImage(urlImage){
          this.default_image = urlImage;
      },
      setDataPicture(data){
          //tampung gambar dari API
          this.productDetails = data;
          //ganti gambar default
          this.default_image = data.galleries[0].photo;
      },
      saveKeranjang(idProduct,nameProduct,priceProduct,photoProduct){
          var productStored = {
              "id" : idProduct,
              "name" : nameProduct,
              "price" : priceProduct,
              "photo" : photoProduct
          }

          this.keranjangUser.push(productStored);
          const parsed = JSON.stringify(this.keranjangUser);
          localStorage.setItem('keranjangUser', parsed);
      }
  },
  mounted: 
  function(){
          if (localStorage.getItem('keranjangUser')) {
            try {
                this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
            } catch(e) {
                localStorage.removeItem('keranjangUser');
            }
        }
        axios.get("http://127.0.0.1:8000/api/products", {params: {id: this.$route.params.id}}).
        then(res=> (this.setDataPicture(res.data.data))).catch(err => console.log(err));
    }
}
</script>
<style scoped>
.pt{
    margin-right: 15px;
}
.product-big-img{
    height: 450px;
    width: 150px;
}
</style>