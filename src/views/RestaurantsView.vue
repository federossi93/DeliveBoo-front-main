<script>
import navBar from '../components/navBar.vue';
import jumbotronRestaurants from '../components/jumbotronRestaurants.vue';
import btnCustomRoundedSmall from '../components/btnCustomRoundedSmall.vue';
import btnCustomRounded from '../components/btnCustomRounded.vue';
import cardRestaurant from '../components/homeComponent/cardRestaurant.vue';
import { store } from '../store';

export default {
    name: 'ReastaurantsView',

    components: {
        navBar,
        jumbotronRestaurants,
        btnCustomRoundedSmall,
        btnCustomRounded,
        cardRestaurant,
    },
    data() {
        return {
            store,
            categories: [
                {
                    url: 'pizza.png',
                    name: 'pizza',
                    id: 1
                },
                {
                    url: 'carne-3.png',
                    name: 'carne',
                    id: 2
                },
                {
                    url: 'pasta.png',
                    name: 'pasta',
                    id: 3
                },
                {
                    url: 'sushi_2.png',
                    name: 'sushi',
                    id: 4
                },
                {
                    url: 'insalate.png',
                    name: 'insalate',
                    id: 5
                },

                {
                    url: 'panini-2.png',
                    name: 'panini',
                    id: 6
                },
                {
                    url: 'hamburger.png',
                    name: 'hamburger',
                    id: 7
                },
                {
                    url: 'piadine-2.png',
                    name: 'piadine',
                    id: 8
                },

                {
                    url: 'fastfood.png',
                    name: 'fastfood',
                    id: 9
                },

                {
                    url: 'messicano.png',
                    name: 'messicano',
                    id: 10
                },
            ],
            filter: [],
        }
    },
    methods: {
        getImageUrl(name) {
            return new URL(`../assets/img/categories/${name}`, import.meta.url).href
        },
    },

}
</script>

<template>
    <navBar />
    <jumbotronRestaurants />

    <div class="container prova">
        <!-- TILTE PAGE -->
        <div class="container title_category">
            <h2>SCEGLI LE TUE CATEGORIE PREFERITE </h2>
        </div>

        <!-- SECTION CATEGORY -->
        <div class="container text-center">
            <!-- ICON CATEGORIES -->
            <div class="row row-cols-4 row-cols-md-6 row-cols-lg-auto justify-content-around">
                <div class="col card_category" v-for="category, i in categories"
                    @click="this.filter.includes(category.name) ? this.filter.splice(filter.indexOf(category.name), 1) : this.filter.push(category.name)"
                    :class="this.filter.includes(category.name) ? 'selected' : ''">
                    <h6 class="py-2">{{ category.name }}</h6>
                    <img :src="getImageUrl(category.url)" alt="">
                </div>
            </div>

            <!-- BTN FILTER CATEGORIES -->
            <btnCustomRounded text="Applica filtri" iconFw="fa-solid fa-utensils" bg_btn="bg_blue"
                bg_hover="hover_blu_light"
                @click="store.callApiRestaurants(store.API_URL + 'api/restaurants/filter/' + this.filter)"
                class="btn_filter ms-1" />
        </div>

        <!-- ELENCO RISTORANTI -->
        <div class="container bg_img my-5 text-start">
            <div class="row row-cols-1 row-cols-md-2 row-cols-lg-4 g-4 ">
                <div class="col my-3 d-flex align-items-stretch" v-for="restaurant in store.restaurants">
                    <div class="card restaurant">
                        <div class="row card_test">
                            <!-- COVER IMAGE -->
                            <div class="col">
                                <div v-if="restaurant.cover_image">
                                    <img class="card-img" :src="store.API_URL + 'storage/' + restaurant.cover_image" alt="">
                                </div>
                                <div v-else>
                                    <img class="card-img" src="../assets/img/placeholder/placehorder_tondo.png"
                                        alt="placeholder">
                                </div>
                            </div>
                            <!-- DETAILS RESTAURANT -->
                            <div class="col">
                                <div class="card-body d-flex flex-column">
                                    <h4 class="card-title">{{ restaurant.restaurant_name }}</h4>
                                    <div class="text"><i class="fa-solid fa-location-dot aspect_ratio me-2"></i>
                                        <span class="text_description">{{ restaurant.address }}</span>
                                    </div>
                                    <div class="text"><i class="fa-regular fa-clock aspect_ratio me-2"></i>
                                        <span class="text_description"> {{ restaurant.opening_time }}</span>
                                    </div>
                                    <div class="text mb-4"><i class="fa-solid fa-bicycle aspect_ratio me-2"></i>
                                        <span class="text_description">{{ restaurant.delivery_price }} €</span>
                                    </div>
                                </div>
                            </div>
                            <!-- BUTTON MENU -->
                            <div class="col">
                                <div class="mx-3 mb-4">
                                    <router-link :to="{ name: 'single-restaurant', params: { slug: restaurant.slug } }">
                                        <btnCustomRoundedSmall text="Menu" iconFw="fa-solid fa-utensils" bg_btn="bg_blue"
                                            bg_hover="hover_blu_light" />
                                    </router-link>
                                </div>
                            </div>
                            <!-- PLATE CATEGORY RESTAURANT -->
                            <div class="col mb-3 mt-3">
                                <div class="categories px-3">
                                    <h5 class="mb-1">Cosa trovi da loro:</h5>
                                    <div class="row" v-for="type in restaurant.types">
                                        <div class="col text-start text_categories"> {{ type.name }} </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<style lang="scss" scoped>
@use '../styles/partials/variables.scss' as *;


.card_test {
    flex-direction: column;
}

.title_category {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 150px;
    border: 3px solid $deliveboo-primary-light;
    background-color: #fcfcfc;
    box-shadow: 3px 3px 9px $deliveboo-primary;
    border-radius: 10px;
    position: relative;
    top: -100px;
    //box-shadow: 3px 3px 9px $deliveboo-dark;
    z-index: 100;
    width: 70%;

    h2 {
        text-align: center;
        font-size: 1.3rem;
        letter-spacing: 0.1rem;
    }

}

.selected {
    background-color: $deliveboo-primary-light;
    color: $deliveboo-white;
    border-radius: 1rem;
    margin-bottom: 1rem;
}


.bg_img {
    // TODO trovare img come background
    background-image: url(../assets/img/);
    background-size: contain;
    background-repeat: no-repeat;
}

h1,
h2 {
    color: $deliveboo-primary;
    word-spacing: 10px;
}

h4 {
    color: $deliveboo-primary;
    font-size: 1.4rem;
}

h5,
.text_categories {
    color: $deliveboo-dark;
}

.text_categories {
    font-size: 1.35rem;
}

.text_description {
    font-size: 1.3rem;
}

.card_category {

    transition: 0.5s;
    padding: 0.5rem;
    margin-right: 1px;


    &:hover {
        cursor: pointer;
        opacity: 50%;
    }

    img {
        width: 100%;
        height: 90px;
        object-fit: contain;
        margin: auto;
        transition: 0.5s;
    }
}

.restaurant {
    background-color: $deliveboo-white;
    box-shadow: 3px 3px 9px $deliveboo-dark;
    //border: 5px solid $deliveboo-primary;
    //border-radius: 10px;

    .text {
        color: $deliveboo-dark;
        font-size: 1.3rem;
    }

    a {
        text-decoration: none;
    }
}

.aspect_ratio {
    aspect-ratio: 1/1;
}

.card-img {
    border-bottom-left-radius: 0;
    border-bottom-right-radius: 0;
    width: 100%;
    aspect-ratio: 1/1;
    object-fit: cover;
}


a {
    background-color: transparent;
}

.btn_filter {
    margin-left: -0.9rem;
    margin-top: 1rem;
}



/* responsive tablet */
@media screen and (max-width: 768px) {
    /*     .prova {
        background-color: lightgreen;
    } */

    h6 {
        font-size: 0.9rem;
    }

    .text_categories {

        font-size: 1.3rem;
    }

    .text_description {
        font-size: 1.3rem;
    }

}

/* responsive mobile */
@media screen and (max-width: 390px) {
    /*     .prova {
        background-color: violet;
    } */

    h6 {
        font-size: 0.7rem;
    }

    .text {
        font-size: 2rem;
        color: red;
    }

    .text_categories {

        font-size: 1.3rem;
    }

    .text_description {
        font-size: 1.4rem;
    }
}
</style>