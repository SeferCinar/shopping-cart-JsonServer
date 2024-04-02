<script>
export default{
    name:"Cart",
    data:()=>{
        return{
            cart: []
        }
    },
    computed:{

    },
    methods:{
        removeItemFromCart(product){
            fetch("http://localhost:3000/cart/" + product.id, {
                    method: "DELETE"
                })
            this.cart.splice(this.cart.indexOf(product),1)
        },
        decrase(product){
            if(product.quantity-1==0){
                fetch("http://localhost:3000/cart/" + product.id, {
                    method: "DELETE"
                })
                this.cart.splice(this.cart.indexOf(product),1)
            }
            else{
                fetch("http://localhost:3000/cart/" + product.id)
                .then(res => res.json())
                .then(data=> {
                    product.quantity = data.quantity - 1
                    console.log(product)
                    fetch("http://localhost:3000/cart/" + product.id, {
                    method: "PATCH",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify(product)
                    })
                
                 })
            }
        },       
        increase(product){
            fetch("http://localhost:3000/cart/" + product.id)
            .then(res => res.json())
            .then(data=> {
                product.quantity = data.quantity + 1
                console.log(product)
                fetch("http://localhost:3000/cart/" + product.id, {
                method: "PATCH",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(product)
                })
            
             })
        },  
        getData(){
            fetch("http://localhost:3000/cart")
            .then(res => res.json())
            .then(data =>{
                this.cart = data
            })
        }      
    },
    beforeMount(){
        this.getData()
    }
}
</script>
<template>
<div>
    <h1>Sepetiniz</h1>
    <div class="cart">
        <div v-for="(product,index) in cart" :key="product.id">
        <img :src=product.imageSource alt="product.name">
        <h3>{{ product.name }}</h3>
        <h4>{{ product.price }}$</h4>
        <div class="countSection">
            <button class = "decrement" v-on:click="decrase(product)">azalt</button>
            <h4>{{ product.quantity }} Adet</h4>
            <button class="increment" v-on:click="increase(product)">arttır</button>
        </div>
        <button v-on:click="removeItemFromCart(product)">Sepetten Kaldır</button>
        </div>
    </div>
</div>
</template>
<style scoped>
.cart{
    display: grid;
    grid-template-columns: 300px 300px 300px 300px;
    grid-template-rows: auto;
    row-gap: 50px;
}
.countSection{
    display: flex;
    justify-content: center;
}
.decrement,.increment {
    height: 50px;
    margin: 10px;
}
</style>