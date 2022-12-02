<template>
    <div class="row card l-fluid-breakout__main">
        <div class="row" v-if="category !== 'men\'s clothing' && category !== 'women\'s clothing'" :class="{card_background: true}">
            <div class="row">
                <div class="col-3"></div>
                <div class="col-6">
                    <span class="u-align--center">
                        This product is unavailable to show
                    </span>
                    <button v-on:click="increment" class="">
                        Next Product
                    </button>
                </div>
            </div>
        </div>
        <div class="row" v-if="category === 'men\'s clothing' || category === 'women\'s clothing'">
            <div class="col-5 viz-grid-item l-fluid-breakout__item">
                <img class="p-image u-align--center" v-bind:src="image" alt="">
            </div>
            <div class="col-6 viz-grid-item l-fluid-breakout__item">
                <div class="row p-divider u-align-text--left">
                    <div class="col-6">
                        <h2 id="title"
                            v-bind:class="{ pria_text: category === 'men\'s clothing', wanita_text: category === 'women\'s clothing'}">
                            {{ title }}</h2>
                        <div class="row u-vertically-center">
                            <div class="col-3">
                                <p id="category">{{ category }}</p>
                            </div>
                            <div class="col-3 u-align-text--right" id="rate">
                                <span>{{ rate }}/5 </span>
                                <span v-if="(rate <= 5)">
                                    <span v-if="category == 'men\'s clothing' ">
                                        <span v-for="n in Math.round(rate)" :key="n">
                                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#002772"
                                                class="bi bi-circle-fill" viewBox="0 0 16 16">
                                                <circle cx="8" cy="8" r="8" />
                                            </svg>
                                        </span>
                                        <span v-for="n in (5 - Math.round(rate))" :key="n">
                                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#002772"
                                                class="bi bi-circle" viewBox="0 0 16 16">
                                                <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z" />
                                            </svg>
                                        </span>
                                    </span>
                                    <span v-if="category == 'women\'s clothing' ">
                                        <span v-for="n in Math.round(rate)" :key="n">
                                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#720060"
                                                class="bi bi-circle-fill" viewBox="0 0 16 16">
                                                <circle cx="8" cy="8" r="8" />
                                            </svg>
                                        </span>
                                        <span v-for="n in (5 - Math.round(rate))" :key="n">
                                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#720060"
                                                class="bi bi-circle" viewBox="0 0 16 16">
                                                <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z" />
                                            </svg>
                                        </span>
                                    </span>
                                </span>
                            </div>
                        </div>
                        <hr>
                    </div>
                    <div class="col-6 u-sv3">
                        <p>{{ description }}</p>
                        <hr>
                    </div>
                    <div class="col-6">
                        <h2 id="price"
                            v-bind:class="{ pria_text: category === 'men\'s clothing', wanita_text: category === 'women\'s clothing'}">
                            ${{ price }}</h2>
                        <div class="row">
                            <div class="col-3 u-align--center button">
                                <button id="buy"
                                    v-bind:class="{ pria_button: category === 'men\'s clothing', wanita_button: category === 'women\'s clothing'}">Buy
                                    Now</button>
                            </div>
                            <div class="col-3 u-align--center button">
                                <button v-on:click="increment" id="next"
                                    v-bind:class="{ pria_border: category === 'men\'s clothing', wanita_border: category === 'women\'s clothing'}">Next
                                    Product</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="col-1 viz-grid-item l-fluid-breakout__item">
            </div>
        </div>
    </div>
    <div class="row">
        <div class="col-12">
        </div>
    </div>
</template>

<script>

import axios from 'axios'

export default {
    name: 'ProductDisplay',
    data() {
        return {
            list: undefined,
            image: "",
            title: "",
            category: "",
            rate: 0,
            description: "",
            price: "",
            count: 0,
        }
    }, 
    methods: {
        getData(){
            axios.get('https://fakestoreapi.com/products/')
                .then((resp) => {
                    this.list = resp.data[this.count];
                    this.image = this.list['image'];
                    this.title = this.list['title'];
                    this.category = this.list['category'];
                    this.rate = this.list['rating']['rate'];
                    this.description = this.list['description'];
                    this.price = this.list['price'];
                    this.bodyBackground(this.category);
                })
        },
        increment() {
            if (this.count <= 18) {
                this.count++
                this.getData();
            } else{
                this.count = 0;
                this.getData();
            }
        },
        bodyBackground() {
            if (this.category == "men's clothing") {
                document.body.classList.remove('body_background_wanita');
                document.body.classList.remove('body_background_lain');
                document.body.classList.add('body_background_pria');
            } else if (this.category == "women's clothing") {
                document.body.classList.remove('body_background_pria');
                document.body.classList.remove('body_background_lain');
                document.body.classList.add('body_background_wanita');
            } else {
                document.body.classList.remove('body_background_pria');
                document.body.classList.remove('body_background_wanita');
                document.body.classList.add('body_background_lain');
            }
        },
    },
    mounted(){
        axios.get('https://fakestoreapi.com/products/')
            .then((resp) => {
                this.list = resp.data[this.count];
                this.image = this.list['image'];
                this.title = this.list['title'];
                this.category = this.list['category'];
                this.rate = this.list['rating']['rate'];
                this.description = this.list['description'];
                this.price = this.list['price'];
                this.bodyBackground(this.category);
                console.warn(this.rate);
            });
    },
}
</script>