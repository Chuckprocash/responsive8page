<script setup>
import { ref, computed, defineProps } from 'vue';
import nostar from '@/assets/images/star-outline.svg';
import halfstar from '@/assets/images/star-half.svg';
import star from '@/assets/images/star-filled.svg';
    
    const props = defineProps({
      premium: {
        type: Boolean,
        required: true,
      },
      varients: {
        type: Array,
        default: () => [],
      },
        details: {
            type: Array,
            required: true,
        },
        reviews: {
          type: Array,
          default: [],
        },
        onSale: {
          type: Boolean,
          default: false,
        }
    });

    const emit = defineEmits(['add-to-cart']);

    let varients = computed(() => props.varients);

  const productImage = computed(() => {
    return varients.value[selectedVarient.value].image;
  });
  const inStock = computed(() => {return varients.value[selectedVarient.value].quantity > 0 ? true : false});
  const inventory = computed(() => {
    return varients.value[selectedVarient.value].quantity;
  });

  const reviewCount = computed(() => {
    let count = 0;
    let averageRating = 0;
    if (props.reviews.length === 0) {
      return 5; // Default to 5 if no reviews
    }else{
    // console.log(props.reviews)
    for (let review of props.reviews){
      // console.log(review);
      count = count + (5 - review.rating);
    }
    averageRating = count / props.reviews.length;
    return 5 - averageRating; 
    }
  });
  const selectedVarient = ref(0);
  const product = ref('Socks');
  const brand = ref('Vue dev8');
  const title = computed(() => brand.value + ' ' + product.value);

  const addToCart = () => {
    if (!inStock.value) {
      alert('Out of Stock');
      return;
    }
    emit('add-to-cart', varients.value[selectedVarient.value].id);
    varients.value[selectedVarient.value].quantity -= 1;
  };
  const setVarient = (index)    => {
    selectedVarient.value = index;
    // console.log(index)
  };
</script>
<template>
    <div class="product-display">
    <div class="product-container">
      <div class="product-image">
        <img :src="productImage" alt="image of green socks" :class="{'outOfStockImg' : !inStock}"/>
      </div>
      <div class="product-info">
        <h1>{{title }}</h1>
        <div class="reviews">
          <div v-for="n in 5" class="review">
            <img v-if="Math.floor(reviewCount) < n && Math.ceil(reviewCount) >= n" :src="halfstar" alt="" />
            <img v-else-if="Math.floor(reviewCount) >= n" :src="star" alt="" />
            <img v-else="Math.floor(reviewCount) < n" :src="nostar" alt="" />
          </div>
        </div>
        <p v-if="inventory > 10">In Stock</p>
        <p v-else-if="inventory <= 10 && inventory > 0">Almost Sold Out!</p>
        <p v-else>Out of Stock</p>
        <p>Shipping: {{premium ? 0.00 : 1.99 }} $</p>
        <ul>
          <li v-for="(detail, index) in details" :key="index">{{ detail }}</li>
        </ul>
        <div class="flex-box">
          <div class="item">

            <div v-for="(varient, index) in varients" :key="varient.id" @mouseover="setVarient(index)" 
            :class="varient===varients[0] ? 'green' : 'blue'" class="color-box"></div>
          </div>
          <div class="item">
            <p v-if="onSale">On Sale!</p>
          </div>
        </div>
        <button @click="addToCart" class="button" :disabled="!inStock">Add To Cart</button>
      </div>
    </div>
  </div>
</template>
<style scoped>
 .product-display {
    max-width: 1100px;
    margin: 0 auto;
  }
  .product-container {
    display: flex;
  }

  .product-image {
    width: 45%;
    margin: 60px;
    display: flex;
    padding: 10px;
    border: 1px solid #ccc;
    justify-content: center;
  }
  .product-image img {
    max-width: 100%;
    box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.1);
  }
  .product-info {
    width: 55%;
    margin: 60px;
    display: flex;
    flex-direction: column;

  }
  .product-info h1 {
    font-size: 2.5em;
    font-weight: bolder;
    margin-bottom: 0;
    font-family: Arial, sans-serif;
  }
  .product-info p {
    font-size: 1em;
    font-weight: 600;
    margin: 10px 0;
    font-family: Arial, sans-serif;
  }
  .product-info ul {
    list-style-type: none;
    padding-left: 25px;
  }
  .product-info li {
    font-size: 1em;
    margin: 5px 0;
    font-family: Arial, sans-serif;
  }

  .button {
    padding: 10px 20px;
    background-color: #121827;
    color: white;
    border: none;
    cursor: pointer;
    font-size: 1em;
    margin-top: 25px;
    width: fit-content;
    margin-left: 25px;
  }
  .button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
  .color-box {
    width: 50px;
    height: 50px;
    cursor: pointer;
    border-radius: 25px;
    border: none;
    margin: 10px;
  }
  .green {
    background-color: #42b983;
  }
  .blue {
    background-color: #42a5f5;
  }
  .color-box:hover {
    border: 2px solid #000;
  }
  .outOfStockImg {
    opacity: 0.5;
  }
  .flex-box{
    display: flex;
  }
  .flex-box .item:nth-of-type(2){
    margin-top: 20px;
    margin-left: 20px;
    font-weight: bold;
    font-size: 2rem;
    color: rgb(255, 28, 28);
  }
  .reviews{
    display: flex;
  }
  .reviews .review{
    height: 30px;
    width: 30px;
    margin: 2px;
  }
  .review img{
    max-width: 100%;
  }
</style>