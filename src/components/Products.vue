<template>
    <div>
        <navbar :cart="cart" :cartQty="cartQty" :cartTotal="cartTotal" @toggle-slide="toggleSlider" @delete-item="deleteItem"></navbar>
        <h1>ARShop</h1>
        <price-slider :sliderStatus="sliderStatus" :maximum.sync="maximum"></price-slider>
        <product-list :products="products" :maximum="maximum" @add-item="addItem"></product-list>
    </div>
</template>

<script>
import ProductList from "./ProductList.vue";
import PriceSlider from "./PriceSlider.vue";
import Navbar from "./Navbar.vue";

export default {
    name:"products",
    props:[
        "products",
        "maximum",
        "cart",
        "cartQty",
        "cartTotal",
        "sliderStatus",
        "sliderState"
    ],
    components:{
        Navbar,PriceSlider,ProductList
    },
    methods:{
        toggleSlider:function(){
            this.$emit('toggle');
        },
        addItem:function(item) {
            this.$emit('add',item);
        },
        deleteItem:function(index) {
            this.$emit('delete',index);
        }
    }
}
</script>