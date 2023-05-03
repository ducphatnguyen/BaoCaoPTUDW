<template>
    <div class="product-card" v-for="(product, index) in products" :key="product._id">
        <img :src="'http://localhost:3000/' + product.image" alt="" />
        <h5 class="title text-center" style="font-size: 20px">{{ product.name }}</h5>
        <div class="price text-danger"
            style=" font-size: 20px; font-weight: bold; font-family: Arial, sans-serif; color: #ff0000;">
            {{ product.price + " VNĐ" }}
        </div>
        <div class="price text-danger"
            style="font-size: 16px; font-weight: normal; font-family: Verdana, sans-serif; color: #000000; ">
            {{ product.quantity_remain === 0 ? "HẾT HÀNG" : "Số lượng kho:" + product.quantity_remain }}
        </div>

        <div class="card-footer text-center">
            <div class="input-group">
                <input type="number" class="form-control" v-model="quantity_sale[index]" min="1" :max="product.quantity_remain" />
                <button class="btn-cart btn " @click="addToCart(
                        {
                            _id: product._id,
                            name: product.name,
                            description: product.description,
                            price: product.price,
                            image: product.image,
                            quantity_sale: quantity_sale[index],
                            category_id: product.category._id,
                        }
                    )" :disabled="product.quantity_remain === 0 || quantity_sale[index] > product.quantity">

                    <i class="fas fa-cart-plus"></i> Thêm vào giỏ
                </button>
            </div>
        </div>
    </div>
</template>
  
<script>
import { defineComponent, ref } from "vue";
import { useRouter } from "vue-router";
import CustomerService from "@/services/customer.service";


export default defineComponent({
    props: {
        products: { type: Array, required: true },
    },

    setup(props, context) {

        const quantity_sale = ref(Array(props.products.length).fill(1));

        const router = useRouter();
        const addToCart = async (data) => {
            console.log(data);
            try {
                await CustomerService.addToCart(data);
                router.push({ name: "cart" });
            } catch (error) {
                console.log(error);
            }
        };
        return {
            addToCart,
            quantity_sale,
        };
    },
});
</script>
<style>
.product-card {
    display: flex;
    flex-direction: column;
    align-items: center;
}
.product-card img {
    width: 150px;
    height: 150px;
    border-radius: 10px 10px 0 0;
}
.product-card .title {
    margin-top: 10px;
}
.product-card .price {
    margin-top: 5px;
}
.product-card .card-footer {
    margin-top: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    background-color: gainsboro;
    border-radius: 0 0 8px 8px;
    height: 60px;
}
.product-card .input-group {
    display: flex;
    flex-direction: row;
    align-items: center;
    margin-top: 10px;
}
.product-card .input-group input {
    width: 50px;
    margin-right: 5px;
    border-radius: 3px;
    border: 1px solid #ced4da;
    padding: 3px 8px;
}
.product-card .input-group button {
    margin-left: 5px;
    border-radius: 3px;
    background-color: #ffa07a;
    color: #fff;
    font-size: 16px;
    padding: 5px 15px;
    transition: background-color 0.2s ease-in-out;
    border: none;
}
.product-card .input-group button:hover {
    background-color: #ff8c61;
}
.btn-cart {
    background-color: #ffa07a;
    color: #fff;
    border-radius: 20px;
    font-size: 18px;
    transition: background-color 0.2s ease-in-out;
}
.btn-cart:hover {
    background-color: #ff8c61;
}
</style>
