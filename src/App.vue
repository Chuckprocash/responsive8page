<script setup>
  import { ref } from 'vue';
  import greenSocks from '@/assets/images/sock-green.jpg';
  import blueSocks from '@/assets/images/sock-blue.jpg';
  import ProductDisplay from './components/ProductDisplay.vue';
  import ReviewForm from './components/ReviewForm.vue';


  const cart = ref([]);
  const premium = ref(true);
  const details = ref(['50% cotton', '30% wool', '20% polyester']);
  const customerReviews = ref([]);
  const onSale = ref(false);
  const varients = ref([
    {
      id: 2234,
      color: 'green',
      image: greenSocks,
      quantity: 10,
    },
    {
      id: 2235,
      color: 'blue',
      image: blueSocks,
      quantity: 0,
    },
  ]);

  const onAddToCart = (id) => {
    cart.value.push(id);
    // console.log(varients.value)
  };
  const onSubmitReview = (review) => {
    customerReviews.value.push(review);
    console.log(customerReviews.value);
  };

</script>

<template>
  <div class="navbar"></div>
  <div class="cart">Cart ({{ cart.length }})</div>
  <ProductDisplay :details="details" :premium="premium" :varients="varients" :reviews="customerReviews" :onSale="onSale" 
  @add-to-cart="onAddToCart" />
  <ReviewForm @review-submitted="onSubmitReview" :reviews="customerReviews"/>
</template>

<style scoped>
  .navbar {
    height: 50px;
    background-color: #42b983;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    margin: 0;
  }

   .cart{
    position: absolute;
    right: 60px;
    top: 60px;
    padding: 15px 20px;
    background-color: #fff;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    border-radius: 5px;
  }
</style>
