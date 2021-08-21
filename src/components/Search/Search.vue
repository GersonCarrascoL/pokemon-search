<template>
    <div>
        <LoadingScreen :isLoading="isLoading" />
        <v-container v-if="!isLoading">
            <v-row justify="center" class="mt-4">
                <v-col cols="12" sm="8" md="8" lg="6">
                    <v-text-field
                        v-model="inputSearch"
                        prepend-inner-icon="mdi-magnify"
                        solo
                        v-on:keyup.enter="onEnter"
                    ></v-text-field>
                </v-col>
            </v-row>
            <pre>{{ inputSearch }}</pre>
            <v-row justify="center" class="content">
                <v-col
                    cols="12"
                    sm="8"
                    md="8"
                    lg="6"
                    v-if="pokemonList.length == 0"
                >
                    <EmptyList></EmptyList>
                </v-col>

                <v-col
                    cols="12"
                    sm="8"
                    md="8"
                    lg="6"
                    v-if="pokemonList.length != 0"
                >
                    lista en 10
                </v-col>
            </v-row>
        </v-container>
    </div>
</template>

<script>
import LoadingScreen from "../../commons/components/LoadingScreen.vue";
import EmptyList from "../Search/PokemnList/EmptyList.vue";

export default {
    data: () => ({
        inputSearch: "",
        isLoading: false,
        pokemonList: [],
    }),
    components: {
        LoadingScreen,
        EmptyList,
    },
    mounted() {
        setTimeout(() => {
            this.isLoading = false;
        }, 3000);
    },
    methods: {
        onEnter: function () {
            this.msg = "on enter event";
            console.log(this.msg + this.inputSearch);
        },
    },
};
</script>
