<template>
    <v-container fill-height>
        <v-row>
            <v-col cols="12">
                <v-row justify="center">
                    <v-col cols="10" sm="8" md="6" lg="4" xl="3">
                        <v-img src="../assets/logo.png"></v-img>
                    </v-col>
                </v-row>
            </v-col>
            <v-col cols="12" class="mt-12">
                <v-row justify="center">
                    <h1 class="home-title">Welcome to Pokedex</h1>
                </v-row>
            </v-col>
            <v-col cols="12" class="mt-5">
                <v-row justify="center">
                    <v-col class="text-center" cols="8">
                        <p class="home-description">
                            The digital encyclopedia created by Professor Oak is
                            an invaluable tool <br />
                            to Trainers in the Pokémon world.
                        </p>
                    </v-col>
                </v-row>
            </v-col>
            <v-col cols="12" class="mt-8">
                <v-row justify="center">
                    <v-btn class="white--text" elevation="2" raised to="/search"
                        >Get Started</v-btn
                    >
                </v-row>
            </v-col>
        </v-row>
    </v-container>
</template>

<style>
.home-title {
    font-size: 26px;
    line-height: 31px;
    letter-spacing: 0em;
    color: #353535;
}

.home-description {
    font-size: 18px;
    line-height: 27px;
    letter-spacing: 0em;
}

.v-btn.v-btn--has-bg {
    background-color: #f22539 !important;
    border-radius: 60px;
    color: white;
}
</style>

<script>
export default {
    data: () => ({
        width: 300,
    }),
};
</script>