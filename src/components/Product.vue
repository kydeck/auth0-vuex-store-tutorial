<template>
    <div class="col mb-4">
        <div class="card">
            <img :src="product.url" class="card-img-top" alt="...">
            <div class="card-body">
            <h5 class="card-title">{{product.name}}</h5>
            <p class="card-text">
                ${{product.price}}
                <br/>
                <small>
                {{product.shortdesc}}
                </small>
            </p>
            <button @click="addToCart()" :disabled="itemAlreadyInCart" class="btn btn-primary">{{ itemAlreadyInCart ? "Added" : "Add to Cart" }}</button>
            </div>
        </div>
    </div>
</template>

<script>
import {mapState} from "vuex";

export default {
    name : "Product",
    props : ['product'],
    computed : {
        ...mapState({
            cart : state => state.cart
        }),
        itemAlreadyInCart(){
            let inCart = false;

            this.cart.forEach(item => {
                if(item.id == this.product.id){
                    inCart = true;
                }
            });

            return inCart;
        }
    },
    methods : {
        // bound to Add to Cart button
        addToCart(){
            // When clicked, it commits the change to the vuex store via mutation, 
            if(!this.itemAlreadyInCart){
                this.$store.commit("addCartItem", this.product);
            }else{
                alert("Item already added to Cart");
            }
        }
    }
}
</script>