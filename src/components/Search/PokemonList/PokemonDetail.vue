<template>
    <div>
        <v-dialog v-model="show" max-width="570px">
            <template v-if="passedObject">
                <v-card>
                    <div style="position: relative">
                        <v-btn
                            class="btn-float-close"
                            x-small
                            fab
                            absolute
                            depressed
                            top
                            right
                            @click.stop="show = false"
                        >
                            <v-icon class="icon-close"> mdi-close </v-icon>
                        </v-btn>
                    </div>
                    <v-row justify="center" class="bg ma-0">
                        <v-col cols="8">
                            <v-img
                                v-if="passedObject"
                                v-bind:src="passedObject.sprites.front_default"
                                max-width="500"
                                max-height="300"
                            >
                                <template v-slot:placeholder>
                                    <v-sheet
                                        color="grey lighten-4"
                                        class="px-3 pt-3 pb-3 fill-height"
                                    >
                                        <v-skeleton-loader
                                            class="mx-auto"
                                            type="image"
                                        ></v-skeleton-loader>
                                    </v-sheet>
                                </template>
                            </v-img>
                        </v-col>
                    </v-row>
                    <v-card-text class="mt-5">
                        <v-row class="mx-0">
                            <p class="detail-description">
                                <strong>Name:</strong>
                                {{ passedObject.nameUpper }}
                            </p>
                        </v-row>
                        <v-row class="mx-0">
                            <v-divider></v-divider>
                        </v-row>
                        <v-row class="mx-0">
                            <p class="detail-description">
                                <strong>Weight:</strong>
                                {{ passedObject.weight }}
                            </p>
                        </v-row>
                        <v-row class="mx-0">
                            <v-divider></v-divider>
                        </v-row>
                        <v-row class="mx-0">
                            <p class="detail-description">
                                <strong>Height:</strong>
                                {{ passedObject.height }}
                            </p>
                        </v-row>
                        <v-row class="mx-0">
                            <v-divider></v-divider>
                        </v-row>
                        <v-row class="mx-0">
                            <p class="detail-description">
                                <strong>Types:</strong>
                                {{ passedObject.typesList }}
                            </p>
                        </v-row>
                    </v-card-text>
                    <v-card-actions>
                        <v-btn
                            :class="[
                                { 'btn-share-small': $vuetify.breakpoint.xs },
                                { 'btn-share-normal': !$vuetify.breakpoint.xs },
                                'white--text',
                                'btn-share',
                            ]"
                            elevation="2"
                            raised
                            @click="copyClipboard"
                            >Share to my friends</v-btn
                        >
                        <v-spacer></v-spacer>
                        <v-btn
                            class="mx-2"
                            fab
                            dark
                            x-small
                            elevation="0"
                            :class="[
                                {
                                    'btn-fav-selected': passedObject.favorite,
                                },
                                'btn-fav',
                            ]"
                        >
                            <v-icon dark> mdi-star </v-icon>
                        </v-btn>
                    </v-card-actions>
                </v-card>
            </template>
        </v-dialog>
        <v-snackbar v-model="snackbar">
            Text copied!
            <template v-slot:action="{ attrs }">
                <v-btn
                    color="pink"
                    text
                    v-bind="attrs"
                    @click="snackbar = false"
                >
                    Close
                </v-btn>
            </template>
        </v-snackbar>
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

.bg {
    background: url("./../../../assets/images/background-pokemon.png");
    height: 100%;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
}

.btn-float-close {
    top: 16px !important;
    color: #88d0f9;
    font-weight: bold !important;
    font-size: 26px;
}

.detail-description {
    margin: 0;
    font-size: 18px;
    line-height: 27px;
    padding: 10px 0px;
    color: #5e5e5e;
}

.btn-share {
    font-weight: 700;
    background-color: #f22539 !important;
    border-radius: 60px;
    color: white;

    height: 44px !important;
    padding: 11px 20px !important;
}

.btn-share-normal {
    font-size: 18px;
}

.btn-share-small {
    font-size: 14px;
}
</style>
<script>
export default {
    data: () => ({
        snackbar: false,
        text: `Text  copied!`,
    }),
    props: {
        value: Boolean,
        passedObject: Object,
    },
    computed: {
        show: {
            get() {
                return this.value;
            },
            set(value) {
                this.$emit("input", value);
            },
        },
    },
    methods: {
        copyClipboard() {
            var clipboard =
                "name: " +
                this.passedObject.name +
                ", weight: " +
                this.passedObject.weight +
                ", height: " +
                this.passedObject.height +
                ", types: " +
                this.passedObject.typesList;
            navigator.clipboard.writeText(clipboard);
            this.snackbar = true;
        },
    },
};
</script>