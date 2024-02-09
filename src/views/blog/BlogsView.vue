
<template>
    <div class="container">
        <div class="row row-cols-1 row-cols-md-2 g-4">
            <div class="col" v-for="product in products">
                <div class="card">
                    <img :src="product.Image" class="card-img-top" alt="...">
                    <div class="card-body">
                        <h5 class="card-title">{{ product.ProductName }}</h5>
                        <p class="card-text">{{ product.RetailPrice }} Ft</p>
                        <a href="/cart" class="btn btn-primary">Kosárba</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import productservices from '../../services/productservices';
import { useUserStore } from '../../stores/userstore';
import { storeToRefs } from 'pinia';
import 'bootstrap'
const { user } = storeToRefs(useUserStore());

const products = ref();

onMounted(async () => {
    products.value = await productservices.getAllProduct()
    console.log(products.value);
    // productservices.getAllProduct()
    // .then(resp => {
    //     // console.log(resp.data);
    //     products.value = resp.data;
    //     console.log(products.value);
    // })
})


</script>

<style lang="scss" scoped>
img {
    width: 150px;
    height: 150px;
}
</style>