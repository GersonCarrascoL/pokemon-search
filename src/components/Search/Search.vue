<template>
    <div>
        <LoadingScreen :isLoading="isLoading" />
        <v-container v-if="!isLoading" class="p-0">
            <v-row justify="center" class="mt-4">
                <v-col cols="12" sm="8" md="8" lg="6" class="pb-0">
                    <v-text-field
                        v-model="inputSearch"
                        label="Search"
                        prepend-inner-icon="mdi-magnify"
                        solo
                        v-on:keyup.enter="onSearchPokemon"
                        clearable
                        @click:clear="onClearClicked"
                    ></v-text-field>
                </v-col>
            </v-row>
            <v-row
                justify="center"
                class="mt-4"
                v-if="alertSuccess || alertError"
            >
                <v-col cols="12" sm="8" md="8" lg="6">
                    <v-alert type="success" :value="alertSuccess">
                        {{ successMessage }}
                    </v-alert>
                    <v-alert type="error" :value="alertError">
                        {{ errorMessage }}
                    </v-alert>
                </v-col>
            </v-row>
            <v-row justify="center" class="content mb-12 mt-0">
                <v-col
                    cols="12"
                    sm="8"
                    md="8"
                    lg="6"
                    v-if="pokemonList.length == 0"
                >
                    <PokemonEmptyList></PokemonEmptyList>
                </v-col>

                <v-col
                    cols="12"
                    sm="8"
                    md="8"
                    lg="6"
                    v-if="pokemonList.length != 0"
                >
                    <div v-for="pokemon in pokemonList" :key="pokemon.name">
                        <v-card
                            elevation="2"
                            class="mt-2 p-5"
                            @click="
                                showPokemonDetailModal({
                                    name: pokemon.name,
                                    favorite: pokemon.favorite,
                                })
                            "
                        >
                            <v-card-text>
                                <v-row align="center">
                                    <v-col
                                        cols="9"
                                        sm="10"
                                        class="card-text-color"
                                    >
                                        {{ pokemon.nameUpper }}
                                    </v-col>
                                    <v-col cols="3" sm="2" class="text-right">
                                        <v-btn
                                            class="mx-2"
                                            fab
                                            dark
                                            small
                                            elevation="0"
                                            :class="[
                                                {
                                                    'btn-fav-selected':
                                                        pokemon.favorite,
                                                },
                                                'btn-fav',
                                            ]"
                                            @click.stop="
                                                changeFavorite(pokemon, $event)
                                            "
                                        >
                                            <v-icon dark size="26">
                                                mdi-star
                                            </v-icon>
                                        </v-btn>
                                    </v-col>
                                </v-row>
                            </v-card-text>
                        </v-card>
                    </div>
                </v-col>
            </v-row>
        </v-container>
        <v-container
            fluid
            class="container-bottom"
            v-if="pokemonList.length != 0"
        >
            <v-container>
                <v-row justify="center">
                    <v-col cols="6" sm="4" md="4" lg="3">
                        <v-btn
                            :class="[
                                { 'btn-select': btnSelectBottom == 'all' },
                                'btn-bottom',
                            ]"
                            @click="changeSelected('all')"
                        >
                            <v-icon left dark size="22">
                                mdi-format-list-bulleted
                            </v-icon>
                            All
                        </v-btn>
                    </v-col>
                    <v-col cols="6" sm="4" md="4" lg="3">
                        <v-btn
                            :class="[
                                { 'btn-select': btnSelectBottom == 'fav' },
                                'btn-bottom',
                            ]"
                            @click="changeSelected('fav')"
                        >
                            <v-icon left dark size="22"> mdi-star </v-icon>
                            Favorites
                        </v-btn>
                    </v-col>
                </v-row>
            </v-container>
        </v-container>
        <PokemonDetail
            v-model="showPokemonDetail"
            :modal.sync="showPokemonDetail"
            :passedObject="objectPassModal"
        />
    </div>
</template>

<style scoped>
.btn-fav {
    color: #bfbfbf !important;
    background: #f5f5f5 !important;
}

.btn-fav-selected {
    color: #eca539 !important;
}

.btn-bottom {
    background-color: #bfbfbf !important;
    width: 100%;
    border-radius: 60px;
    color: white;
    font-size: 18px;
    height: 44px !important;
}

.btn-select {
    background-color: #f22539 !important;
    color: white;
}

.container-bottom {
    padding: 0;
    margin: 0;
    width: 100%;
    height: 64px;
    position: fixed;
    bottom: 0;
    background: white;
    box-shadow: 0px -5px 4px rgba(0, 0, 0, 0.05);
}

.card-text-color {
    font-size: 22px;
    line-height: 26px;
    letter-spacing: 0em;
    color: #353535;
}
</style>
<script>
import { RepositoryFactory } from "../../repositories/RepositoryFactory";
const PokemonRepository = RepositoryFactory.get("pokemon");

import LoadingScreen from "../../commons/components/LoadingScreen.vue";
import PokemonEmptyList from "./PokemonList/PokemonEmptyList.vue";
import PokemonDetail from "./PokemonList/PokemonDetail.vue";

export default {
    data: () => ({
        inputSearch: "",
        isLoading: true,
        alertSuccess: false,
        alertError: false,
        successMessage: "",
        errorMessage: "",
        pokemonList: [],
        showPokemonDetail: false,
        objectPassModal: null,
        btnSelectBottom: "all",
    }),
    components: {
        LoadingScreen,
        PokemonEmptyList,
        PokemonDetail,
    },
    beforeMount() {
        this.getListPokemon();
    },
    mounted() {
        setTimeout(() => {
            this.isLoading = false;
        }, 2000);
    },
    methods: {
        async showPokemonDetailModal(item) {
            let { data } = await PokemonRepository.getPokemon(item.name);
            data["nameUpper"] =
                data.name.charAt(0).toUpperCase() + data.name.slice(1);

            data.types.forEach((type, index) => {
                data["typesList"] = "";
                index + 1 == data.types.length
                    ? (data["typesList"] += type.type.name + "")
                    : (data["typesList"] += type.type.name + ",");
            });
            data["favorite"] = item.favorite;
            this.objectPassModal = data;
            this.showPokemonDetail = true;
        },
        getFavorites() {
            var favorites = this.pokemonList.filter(
                (pokemon) => pokemon.favorite == true
            );
            this.pokemonList = favorites;
        },
        changeFavorite: function (pokemon) {
            pokemon.favorite = !pokemon.favorite;
            let favorites = JSON.parse(
                localStorage.getItem("favorites") || "[]"
            );
            if (pokemon.favorite) {
                if (favorites) {
                    favorites.push({
                        name: pokemon.name,
                    });
                }
            } else {
                favorites.forEach((pokemonFav, index, object) => {
                    if (pokemonFav.name == pokemon.name) {
                        object.splice(index, 1);
                    }
                });
            }
            localStorage.setItem("favorites", JSON.stringify(favorites));
            this.$forceUpdate();
        },
        onClearClicked() {
            this.getListPokemon();
        },
        changeSelected(value) {
            this.btnSelectBottom = value;
            this.inputSearch = "";
            switch (value) {
                case "fav":
                    this.getFavorites();
                    break;
                case "all":
                    this.getListPokemon();
                    break;
            }
        },
        async getListPokemon() {
            try {
                let { data } = await PokemonRepository.getListPokemon();
                if (data != undefined) {
                    this.successMessage = "Successfull search!.";
                    this.pokemonList = data.results;

                    let favorites = JSON.parse(
                        localStorage.getItem("favorites") || "[]"
                    );
                    this.pokemonList.forEach((pokemon) => {
                        let find = favorites.find(
                            (pokemonFav) => pokemonFav.name == pokemon.name
                        );
                        find
                            ? (pokemon["favorite"] = true)
                            : (pokemon["favorite"] = false);

                        pokemon["nameUpper"] =
                            pokemon.name.charAt(0).toUpperCase() +
                            pokemon.name.slice(1);
                    });
                } else {
                    this.successMessage = "Nothing found";
                    this.pokemonList = [];
                }
            } catch (error) {
                this.isLoading = false;
                this.alertSuccess = false;
                this.alertError = true;
                this.errorMessage = error;
                setTimeout(() => {
                    this.alertError = false;
                }, 2000);
            }
        },
        async onSearchPokemon() {
            try {
                this.isLoading = true;

                let data = await PokemonRepository.getPokemon(
                    this.inputSearch.toLowerCase().trim()
                );
                this.isLoading = false;
                this.alertSuccess = true;
                this.alertError = false;
                if (data != undefined) {
                    this.successMessage = "Successfull search!.";
                    this.pokemonList = [
                        {
                            name: data.data.name,
                            nameUpper:
                                data.data.name.charAt(0).toUpperCase() +
                                data.data.name.slice(1),
                        },
                    ];
                } else {
                    this.successMessage = "Nothing found";
                    this.pokemonList = [];
                }
                setTimeout(() => {
                    this.alertSuccess = false;
                }, 2000);
            } catch (error) {
                this.pokemonList = [];
                this.isLoading = false;
                this.alertSuccess = false;
                this.alertError = true;
                this.errorMessage = error;
                setTimeout(() => {
                    this.alertError = false;
                }, 5000);
            }
        },
    },
};
</script>
