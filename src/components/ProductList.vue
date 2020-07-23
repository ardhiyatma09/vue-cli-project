<template>
    <transition-group name="custom2" tag="div" @beforeEnter="before" @enter="enter" @leave="leave">
        <div class="row mb-3 align-items-center" v-for="(item,index) in showItem" :key="item.id" :data-index="index">
            <div class="col-1 m-auto">
                <button class="btn btn-info" v-on:click.stop="$emit('add-item',item)">+</button>
            </div>
            <div class="col-sm-4">
                <img class="img-fluid" :src="item.image" :alt="item.image_title">
            </div>
            <div class="col">
                <h3 class="text-info">{{item.name}}</h3>
                <p class="mb-0">{{item.description}}</p>
                <div class="h5 float-right">
                    <price :value="Number(item.price)"></price>
                </div>
            </div>
        </div>
    </transition-group>
</template>

<script>
import Price from "./Price.vue";

export default {
    name:"product-list",
    components: {
        Price
    },
    props: ['products','maximum'],
    computed:{
        showItem:function(){
            let max = this.maximum      
            return this.products.filter(function(item){
                return item.price <= Number(max);
            })
        }
    },
    methods: {
        addItem:function(product){ 
            var productIndex;
            var productExist = this.cart.filter(function(item,index){
                if(item.product.id == Number(product.id)){
                    productIndex = index;
                    return true;
                } else {
                    return false;
                }
            });

            if(productExist.length){
                this.cart[productIndex].qty++
            } else {
                this.cart.push({product: product, qty : 1});
            }
        }, // function untuk add product ke dalam cart dan dengan mengecek qty 
        before:function(el){
            el.className = 'd-none';
        }, // function untuk animasi product sebelum muncul
        enter:function(el){
            var delay = el.dataset.index * 100;
            setTimeout(function(){
                el.className = 'row d-flex mb-3 align-items-center animate__animated animate__flipInX'
            }, delay) // function untuk animasi product ketika muncul
        },
        leave:function(el){
            var delay = el.dataset.index * 100;
            setTimeout(function(){
                el.className = 'row d-flex mb-3 align-items-center animate__animated animate__slideOutRight'
            }, delay) // function untuk animasi product ketika keluar
        },
        deleteItem:function(key){
            if (this.cart[key].qty > 1) {
                this.cart[key].qty--;
            } else {
                this.cart.splice(key,1);
            }
        }
    },
}
</script>