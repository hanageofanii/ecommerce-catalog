<script>
export default {
    name: 'ProductDisplay',
    data() {
        return {
            product: {},
            index: 1,
            theme: {},
            status: false,
            isAvailable: true,
        };
    },
    created() {
        this.status = true;
        this.fetchProducts();
    },
    methods: {
        async fetchProducts() {
            const res = await fetch(
                `https://fakestoreapi.com/products/${this.index}`
            );
            const productData = await res.json();
            this.product = productData;
            this.changeTheme();

            const { category } = this.product;

            if (
                category === "men's clothing" ||
                category === "women's clothing"
            ) {
                this.isAvailable = true;
            } else {
                this.isAvailable = false;
            }

            this.status = false;
        },
        fetchNextProduct() {
            this.status = true;
            if (this.index === 20) {
                this.index = 1;
            } else {
                this.index++;
            }
            this.fetchProducts();
        },
        changeTheme() {
            switch (this.product.category) {
                case "men's clothing":
                    this.theme = {
                        bg: 'bg-men',
                        text: 'text-men',
                        rating: 'circle-men',
                        filled: 'filled-men',
                        buttonBuy: 'btn-buy-men',
                        buttonNext: 'btn-next-men',
                    };
                    break;
                case "women's clothing":
                    this.theme = {
                        bg: 'bg-women',
                        text: 'text-women',
                        rating: 'circle-women',
                        filled: 'filled-women',
                        buttonBuy: 'btn-buy-women',
                        buttonNext: 'btn-next-women',
                    };
                    break;
                default:
                    this.theme = {
                        bg: 'bg-default',
                    };
                    break;
            }
        },
    },
};
</script>

<template>
    <div class="bg" :class="theme.bg"></div>
    <section class="wrapper">
        <div class="product-box">
            
            <div v-if="status">
                <div class="loading">
                    <div class="img"></div>
                    <div class="load-detail">
                        <div class="load-text">
                            <div class="load-title"></div>
                            <div class="load-desc">
                                <div></div>
                                <div></div>
                                <div></div>
                                <div></div>
                            </div>
                        </div>
                        <div class="load-btn">
                            <div></div>
                            <div></div>
                        </div>
                    </div>
                </div>
            </div>

            <div v-if="!status && isAvailable" class="product-wrapper">
                <div class="img-wrapper">
                    <img :src="product.image" :alt="product.title" />
                </div>
                <div class="wrapper-col">
                    <div class="detail-wrapper">
                        <h1 :class="theme.text">{{ product.title }}</h1>

                        <div>
                            <div class="category">
                                <span>{{ product.category }}</span>
                                <div class="rating">
                                    <span>{{ product?.rating?.rate }}/5</span>
                                    <div class="rating-circle">
                                        <div v-for="index in 5" :key="index" class="circle" :class="[
                                        theme.rating,
                                        {
                                            [theme.filled]:
                                            index <=
                                            product?.rating?.rate,
                                        },
                                        ]"></div>
                                    </div>
                                </div>
                            </div>
                            <hr />
                        </div>

                        <p>{{ product.description }}</p>
                    </div>
                    <div class="detail-wrapper">
                        <div>
                            <hr/>
                            <span :class="theme.text" class="price">${{ product.price }}</span>
                        </div>
                        <div class="btns-wrapper">
                            <button :class="theme.buttonBuy">Buy Now</button>
                            <button :class="theme.buttonNext" @click="fetchNextProduct">
                                Next Product
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <div v-else class="product-wrapper">
                <div class="sad-img">
                    <img src="../assets/sad-face.svg" alt="Sad Face" />
                </div>
                <div class="sad-wrapper">
                    <p>This product is unavailable to show</p>
                    <button @click="fetchNextProduct">Next Product</button>
                </div>
            </div>
        </div>
    </section>
</template>
