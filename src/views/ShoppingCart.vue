<template>
<div class="shopping">
    <headerAldi />
        <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12 text-left">
                    <div class="breadcrumb-text product-more">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                        <span>Shopping Cart</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
        <div class="container" v-if="keranjangUser.length > 0">
            <div class="row">
                <div class="col-lg-8">
                    <div class="row">
                        <div class="col-lg-12">
                            
                            <div class="cart-table">
                                <table>
                                    <thead>
                                        <tr>
                                            <th>Image</th>
                                            <th class="p-name text-center">Product Name</th>
                                            <th>Price</th>
                                            <th>Action</th>
                                        </tr>
                                    </thead>

                                        <tbody v-for="keranjang in keranjangUser" :key="keranjang.id">
                                            
                                                <tr>
                                                    <td class="cart-pic first-row">
                                                        <img :src="keranjang.photo" class="img-cart" />
                                                    </td>
                                                    <td class="cart-title first-row text-center">
                                                        <h5>{{keranjang.name}}</h5>
                                                    </td>
                                                    <td class="p-price first-row">Rp{{keranjang.price}}</td>
                                                    <td class="delete-item"><a @click="removeItem(keranjangUser.index)" href="#"><i class="material-icons">
                                                    close
                                                    </i></a></td>
                                                </tr>
                                        </tbody>
                                </table>
                            </div>

                        </div>
                        <div class="col-lg-8">
                            <h4 class="mb-4">
                                Informasi Pembeli:
                            </h4>
                            <div class="user-checkout text-left">
                                <form>
                                    <div class="form-group">
                                        <label for="namaLengkap">Nama lengkap</label>
                                        <input type="text" class="form-control" id="namaLengkap" aria-describedby="namaHelp" placeholder="Masukan Nama" v-model="customerInfo.name" >
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">Email Address</label>
                                        <input type="email" class="form-control" id="emailAddress" aria-describedby="emailHelp" placeholder="Masukan Email" v-model="customerInfo.email" >
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">No. HP</label>
                                        <input type="text" class="form-control" id="noHP" aria-describedby="noHPHelp" placeholder="Masukan No. HP" v-model="customerInfo.number">
                                    </div>
                                    <div class="form-group">
                                        <label for="alamatLengkap">Alamat Lengkap</label>
                                        <textarea class="form-control" id="alamatLengkap" rows="3" v-model="customerInfo.address"></textarea>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="proceed-checkout text-left">
                                <ul>
                                    <li class="subtotal">ID Transaction <span>#SH12000</span></li>
                                    <li class="subtotal mt-3">Subtotal <span>Rp.{{totalPrice}}</span></li>
                                    <li class="subtotal mt-3">Pajak <span>Rp.{{tax}}</span></li>
                                    <li class="subtotal mt-3">Total Biaya <span>Rp.{{total}}</span></li>
                                    <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                                    <li class="subtotal mt-3">No. Rekening <span>2208 1996 1403</span></li>
                                    <li class="subtotal mt-3">Nama Penerima <span>Shayna</span></li>
                                </ul>
                                <!-- <router-link to="/success">I ALREADY PAID</router-link> -->
                                <a @click="checkout" href="#"  class="proceed-btn">I Already Paid</a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div v-else class="mb-4">
            <h3> No Data! </h3>
        </div>

    </section>
    <!-- Shopping Cart Section End -->
    <footerAldi />
</div>
</template>

<script>
import headerAldi from '@/components/headerAldi.vue';
import footerAldi from '@/components/footerAldi.vue';
import axios from 'axios';
export default {
    name: 'ShoppingCart',

    data(){
            return{
                keranjangUser: [],
                customerInfo: {
                    name: '',
                    email: '',
                    number: '',
                    address: ''
                }
            }
        },
        methods: {
            removeItem(index){
                this.keranjangUser.splice(index,1);
                const parsed = JSON.stringify(this.keranjangUser);
                localStorage.setItem('keranjangUser', parsed);
            },
            checkout(){
                let productId = this.keranjangUser.map(function(product){
                    return product.id;
                });
                let checkData = {
                    'name' : this.customerInfo.name,
                    'email' : this.customerInfo.email,
                    'number' : this.customerInfo.number,
                    'address' : this.customerInfo.address,
                    'total' : this.total,
                    'status' : 'PENDING',
                    'transaction_details' : productId
                }
                axios.post("http://127.0.0.1:8000/api/checkout",checkData).then(() => this.$router.push('success'))
                .catch(err => console.log(err));
            }
        },
        mounted(){
            if (localStorage.getItem('keranjangUser')) {
                try {
                    this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
                } catch(e) {
                    localStorage.removeItem('keranjangUser');
                }
            }
        },
        // menghitung total harga
        computed: {
            totalPrice(){
                return this.keranjangUser.reduce(function(items,data){
                    return items + data.price;
                },0);
            },
            tax(){
                return this.totalPrice*10/100;
            },
            total(){
                return this.totalPrice + this.tax;
            }
        },
    components:{
        headerAldi,
        footerAldi,
    }    
}
</script>

<style scoped>
.img-cart{
    width: 100px;
    height: 100px;
}
</style>