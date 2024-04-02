<script>
  export default {
    data: () => {
      return{
        page:"products",
      }
    },
    components:{
      Products:Products,
      Cart:Cart
    },
    methods:{
      navigateTo(page){
        this.page = page
      }
      
    }
  }
  import Products from './components/Products.vue'
  import Cart from './components/Cart.vue'
</script>

<template>
 <div>

  <div class="navbar">
    <button class="navbarBtn" v-on:click="navigateTo('products')">Alışverişe devam et</button> 
    <button class="navbarBtn" v-on:click="navigateTo('cart')">Sepete git</button>
  </div>

  <div v-if="this.page == 'products'">
  <Products/>
  </div>
  <div v-if="this.page == 'cart'">
  <Cart/>
  </div>
 </div>
</template>

<style scoped>
.navbarBtn{
  margin-left: 200px;
  margin-right: 200px;
}
</style>
