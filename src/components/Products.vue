<script>
export default{
    data: () => {
        return{
            products:[
                
            ],
        
            
        };
    },
    methods:{
        addItemToCart(product,quantityProp){       
            
            fetch("http://localhost:3000/cart/" + product.id)
            .then(response => {
                if (response.ok) {
                    fetch("http://localhost:3000/cart/" + product.id)
                    .then(res => res.json())
                    .then(data=> {
                        product.quantity = data.quantity + quantityProp
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
                else {
                    product.quantity = quantityProp
                    fetch("http://localhost:3000/cart", {
                        method:"POST",
                        headers: {
                        'Content-Type': 'application/json'
                        },
                        body: JSON.stringify(product)
                    })
                }})
        },
        getData(){
            fetch("http://localhost:3000/products")
            .then(res => res.json())
            .then(data => {
                this.products = data
            }
                
            )
        }
    },
    beforeMount(){
        this.getData()
    }
}

</script>
<template>
<div>
    <h1>Ürünler</h1>
    <div class="products">
        <div v-for="(product) in products" :key="product.id">
         <img :src=product.imageSource alt="product.name">
         <h3>{{ product.name }}</h3>
         <h4>{{ product.price }}</h4>
         <label for="quantity">Adet:</label>
         <input class="input-number" type="number" :id=product.id min="1" v-model="zort" value="1"/> 
         <button class="button-add-to-cart" v-on:click="addItemToCart(product,zort)">Sepete Ekle</button>
        </div>
    </div>
</div>
</template>
<style scoped>
.products{
    display: grid;
    grid-template-columns: 300px 300px 300px 300px;
    grid-template-rows: auto;
    row-gap:50px
}
/* Input alanı ve arttırma/azaltma düğmeleri için stil */
input[type="number"] {
    font-size: 30px;
  width: 50px; /* Input alanının genişliği */
  height: 50px;
  padding: 5px; /* Input alanının iç kenar boşluğu */
  border: 1px solid #ccc; /* Input alanının kenarlık rengi */
  border-radius: 4px; /* Input alanının köşe yuvarlaklığı */
}
/* Input alanı odaklandığında kenarlık rengini değiştir */
input[type="number"]:focus {
  border-color: #007bff; /* Kenarlık rengi: mavi */
}
</style>