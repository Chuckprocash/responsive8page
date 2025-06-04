<script setup>
    import { ref } from 'vue';

    const name = ref('');
    const review = ref('');
    const rating = ref('');

    const emit = defineEmits(['review-submitted']);
    const props = defineProps({
        reviews: {
            type: Array,
            default: () => []
        }
    });

    const submitReview = () => {
        if (name.value && review.value && rating.value) {
            alert(`Thank you for your review, ${name.value}!`);
            const reviewData = {
                name: name.value,
                review: review.value,
                rating: rating.value
            };
            emit('review-submitted', reviewData);
            // Reset the form
            name.value = '';
            review.value = '';
            rating.value = '';
        } else {
            alert('Please fill out all fields.');
        }
    };
</script>
<template>
    <div class="container">
        <div class="item">
            <form class="review-form">
                <h3>Write a review</h3>
                <div class="form-group">
                    <label for="name">Name:</label>
                    <input type="text" id="name" v-model="name" required />
                </div>
                <div class="form-group">
                    <label for="review">Review:</label>
                    <textarea id="review" v-model="review" required></textarea>
                </div>
                <div class="form-group">
                <label for="rating">Rating:</label>
                <select id="rating" v-model="rating" required>
                    <option value="" disabled>Select a rating</option>
                    <option v-for="n in 5" :key="n" :value="n">{{ n }} Star{{ n > 1 ? 's' : '' }}</option>
                </select>
                </div>
                <button type="submit" @click.prevent="submitReview">Submit Review</button>
            </form>
        </div>
        <div class="review-item">
            <h2>Customer Reviews</h2>
            <ul v-if="reviews.length > 0">
                <li v-for="(review, index) in reviews" :key="index">
                    <strong>{{ review.name }}</strong> ({{ review.rating }} stars): {{ review.review }}
                </li>
            </ul>
        </div>
    </div>
</template>
<style scoped>
.container{
    max-width: 1100px;
    margin: 0 auto;
    display: flex;
}
.review-form {
    max-width: 400px;
    padding: 20px;
    margin: 60px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #f9f9f9;
}
form input,textarea,select {
    width: 100%;
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
}
form button {
    padding: 10px 15px;
    background-color: #42b983;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}
form button:hover {
    background-color: #36a074;
}
form h3 {
    margin-bottom: 20px;
    font-size: 1.5em;
    font-weight: bold;
}
.container .item{
    flex: 1;
}
.review-item {
    margin: 60px;
    flex: 1;
}
.review-item h2 {
    font-size: 2em;
    margin-bottom: 20px;
}
.review-item ul {
    list-style-type: none;
    padding-left: 0;
}
.review-item li {
    margin-bottom: 15px;
    padding: 10px;
}
@media(max-width: 950px){
    .container{
    margin: 40px auto;
    flex-direction: column;
    justify-content: center;
}
.review-form{
    margin: 10px 5px;
}
.container .item{
    display: flex;
    justify-content: center;
}
.review-form{
    width:80%;
}
}
</style>