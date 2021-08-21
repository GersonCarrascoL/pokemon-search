<template>
    <v-container>
        <v-row justify="center" class="mt-3">
            <h2 class="empty-list-title">Uh-oh!</h2>
        </v-row>
        <v-row justify="center" class="mt-5">
            <p class="empty-list-text">You look lost on your journey!</p>
        </v-row>
        <v-row justify="center" class="mt-5">
            <v-btn class="white--text" elevation="2" raised to="/"
                >Go back home</v-btn
            >
        </v-row>
    </v-container>
</template>

<style scoped>
.empty-list-title {
    font-size: 36px;
    line-height: 43px;
    letter-spacing: 0em;
}

.empty-list-text {
    font-size: 20px;
    line-height: 30px;
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
    name: "EmptyList",
};
</script>