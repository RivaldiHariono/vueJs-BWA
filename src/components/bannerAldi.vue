<template>
        <!-- Women Banner Section Begin -->
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :nav=false :autoplay=true :items=3> 
                        <div class="product-item" v-for="itemProduct in products" :key="itemProduct.id">
                            <div class="pi-pic" v-if="itemProduct.galleries.length > 0">
                                <img v-bind:src="itemProduct.galleries[0].photo" alt="" height="330px" />
                                <ul>
                                    <router-link to="/shoppingcart">
                                    <li class="w-icon active">
                                        
                                        <a  @click="saveKeranjang(itemProduct.id,itemProduct.name,itemProduct.price,itemProduct.galleries[0].photo)" href="#"><i class="icon_bag_alt"></i></a>
                                        
                                    </li>
                                    </router-link>
                                    <li class="quick-view"><router-link v-bind:to="'/product/'+itemProduct.id">+ Quick View</router-link></li>
                                    
                                </ul>
                            </div>
                            <div class="pi-pic" v-else>
                                <img alt="Foto Belum Tersedia" height="330px" />
                                <ul>
                                    <li class="w-icon active">
                                        <router-link to="/shoppingcart">
                                        <a @click="saveKeranjang(itemProduct.id,itemProduct.name,itemProduct.price,itemProduct.galleries[0].photo)" href="#" class="primary-btn pd-cart"><i class="icon_bag_alt"></i></a>
                                        </router-link>
                                    </li>
                                    <li class="quick-view"><router-link v-bind:to="'/product/'+itemProduct.id">+ Quick View</router-link></li>
                                    
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{itemProduct.type}}</div>
                                <a href="#">
                                    <h5>{{itemProduct.name}}</h5>
                                </a>
                                <div class="product-price">
                                    {{itemProduct.price}}
                                    <span>$35.00</span>
                                </div>
                            </div>
                        </div>

                    </carousel>
                </div>
                <div class="col-lg-12 mt-5" v-else>
                    <p>Data Null</p>
                </div>
            </div>
        </div>
    </section>
    <!-- Women Banner Section End -->
</template>

<script>
import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
    name: 'bannerAldi',
    components: {
        carousel
    },
    data(){
        return {
            products: [],
            keranjangUser: []
        };
    },
    mounted: function(){
        if (localStorage.getItem('keranjangUser')) {
            try {
                this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
            } catch(e) {
                localStorage.removeItem('keranjangUser');
            }
        }
        axios.get("http://127.0.0.1:8000/api/products").then(res=> (this.products = res.data.data.data)).catch(err => console.log(err));
    },
    methods:{
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
    }
}
</script>

<style scoped>
.product-item{
    margin-right: 25px;
}
</style>