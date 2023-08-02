<template>
    <div class="full-content">
        <headNav></headNav>
        <div class="row">
            <sideBar></sideBar>
            <div class="col-md-10 main-content p-0 m-0">
                <!-- The floating button -->
                <button v-show="!isFloatingDivVisible" class="floating-button rounded border border-1 d-none d-md-block"
                    @click="showFloatingDiv">
                    <!-- Content of the button (bag icon and item count) -->
                    <div class="bg-warning p-2 text-center rounded">
                        <div class="icon text-warning">
                            <img alt="bag Icon" class="img-fluid" :src="getUrl('public/images/others/bag.svg')" />
                        </div>
                        <b v-if="carts">{{ carts.filter(cart => cart.is_buy_now == false).length }} Items</b>
                    </div>
                    <!-- Cart total amount -->
                    <div class="bg-secondary p-2 text-center rounded">
                        <b class="text-white">
                            <transition name="counter" mode="out-in">
                                <div class="counter" :key="cartTotal">{{ priceFormat(cartTotal) }}</div>
                            </transition>
                        </b>
                    </div>
                </button>

                <!-- Floating div (conditional rendering) -->
                <div v-if="isFloatingDivVisible" class="floating-div">
                    <div class="d-flex justify-content-between mb-2">
                        <!-- Bag Icon -->
                        <h3><i class="mdi mdi-cart"></i></h3>

                        <!-- Number of items in the cart -->
                        <b v-if="carts">{{ carts.filter(cart => cart.is_buy_now == false).length }} Items</b>

                        <!-- Close button for the floating div -->
                        <button class="btn btn-sm btn-danger" @click="hideFloatingDiv">Close</button>
                    </div>
                    <table class="table bg-white">
                        <tbody v-if="carts">
                            <tr v-for="(cart, index) in carts" :key="index">
                                <td>
                                    <div class="btn-group">
                                        <!-- <button class="btn btn-sm btn-danger border border-1"
                                            @click="cartMinus(index)">-</button> -->
                                        <button class="btn btn-sm"><b class="text-success">{{ cart.quantity }}</b></button>
                                        <!-- <button class="btn btn-sm btn-success border border-1"
                                            @click="cartBtn(productDetails(cart.product_slug), index)">+</button> -->
                                    </div>
                                </td>
                                <td>
                                    <router-link :to="{ name: 'product.details', params: { slug: cart.product_slug } }">
                                        <img loading="lazy" :src="cart.image_40x40" :alt="cart.product_name"
                                            class="img-fluid" />
                                    </router-link>
                                </td>
                                <td>
                                    <router-link :to="{ name: 'product.details', params: { slug: cart.product_slug } }">
                                        {{ cart.product_name }}
                                    </router-link>
                                </td>
                                <td>{{ priceFormat((cart.price - cart.discount) * cart.quantity) }} </td>
                                <td>
                                    <button class="btn btn-sm btn-danger" @click="deleteCart(cart.id)">
                                        <i class="mdi mdi-trash-can-outline"></i>
                                    </button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    <div class="text-center buttons">
                        <router-link :to="{ name: 'cart' }" class="btn btn-primary">{{ lang.view_cart }}</router-link>
                        <router-link :to="{ name: 'checkout' }" class="btn btn-primary" @click="checkoutPage($event)">{{
                            lang.check_out }}</router-link>
                    </div>
                </div>

                <router-view></router-view>
                <bottom></bottom>
            </div>
        </div>
    </div>
</template>
  
<script>
import headNav from "./partials/header";
import sideBar from "./partials/sidebar";
import bottom from "./partials/bottom";

export default {
    name: "master",

    components: {
        headNav,
        bottom,
        sideBar,
    },

    data() {
        return {
            prevCartTotal: 0,
            isFloatingDivVisible: false,
            products: null
        };
    },

    watch: {
        cartTotal(newValue, oldValue) {
            this.prevCartTotal = oldValue;
        },
    },

    mounted() {
        document.title =
            this.$route.meta.title && typeof this.$route.meta.title != "undefined"
                ? this.$route.meta.title
                : this.settings.system_name;
        this.products = this.$store.getters.getProductDetails;
    },

    computed: {
        carts() {
            return this.$store.getters.getCarts;
        },
        cartTotal() {
            var cartProducts = this.$store.getters.getCarts;
            var total = 0;

            if (cartProducts) {
                for (var i = 0; i < cartProducts.length; i++) {
                    total += cartProducts[i].quantity * (cartProducts[i].price - cartProducts[i].discount);
                }
            }
            return total;
        },
    },

    methods: {
        showFloatingDiv() {
            this.isFloatingDivVisible = true; // Show the floating div when the button is clicked
        },

        hideFloatingDiv() {
            this.isFloatingDivVisible = false; // Hide the floating div when the close button is clicked
        },
        cartMinus(index) {
            var carts = this.$store.getters.getCarts;
            if (carts && carts[index].quantity > 1) {
                let formData = {
                    id: carts[index].id,
                    quantity: -1,
                    status: 'minus',
                };

                let url = this.getUrl('cart/update');

                axios.post(url, formData).then((response) => {
                    this.$store.dispatch('carts', response.data.carts);
                })
            }
            else {
                this.deleteCart(carts[index].id);
            }
        },
        productDetails(product_slug) {
            for (let i = 0; i < this.products.length; i++) {
                if (products[i].slug === product_slug) {
                    return products[i].product;
                } else {
                    console.log(product_slug + 'x');
                }
            }
            return false;
        },
        deleteCart(id) {
            let url = this.getUrl('cart/delete/' + id);
            axios.get(url).then((response) => {
                this.$store.dispatch('carts', response.data.carts);
            })
        },
    },
};
</script>
  
<style scoped>

.full-content {
    height: 100vh;
    overflow-y: hidden;
    overflow-x: hidden;
}
.main-content {
    height: 90vh;
    overflow-y: auto;
    overflow-x: hidden;
}

.floating-button {
    position: fixed;
    bottom: 50%;
    right: 20px;
    z-index: 999;
    border-radius: 2px;
    border: 1px;
    opacity: 0.7;
    cursor: pointer;
}

.floating-button:hover {
    opacity: 1;
}

/* Optional: Add a transition effect for the button */
.floating-button button {
    transition: transform 0.1s ease;
}

/* Optional: Apply a slight hover effect for the button */
.floating-button button:hover {
    transform: scale(1.1);
}

.floating-div {
    position: fixed;
    top: 30%;
    right: 20px;
    transform: translateY(-50%);
    background-color: #f0f0f0;
    border: 1px solid #ccc;
    padding: 10px;
    border-radius: 5px;
    z-index: 999;
    /* pointer-events: none; */
    /* Remove this line to make the buttons clickable */
    transition: opacity 0.3s ease-in-out;
}

.floating-div.active {
    opacity: 1;
    /* pointer-events: auto; */
    /* Remove this line to make the buttons clickable */
}

.counter {
    display: inline-block;
}

/* Animated Counter Effect */
.counter-enter-active,
.counter-leave-active {
    animation: bounce 0.5s;
}

@keyframes bounce {
    0% {
        transform: scale(1);
    }

    50% {
        transform: scale(1.5);
    }

    100% {
        transform: scale(1);
    }
}

/* Optional: Apply a slight hover effect for the cart counter */
.cart-counter:hover {
    transform: scale(1.1);
}
</style>
  