<template>
    <div>
        <v-container>
            <v-form>
                <v-file-input
                    @change='fileUploaded'
                    multiple
                    accept="image/png, image/jpeg, image/bmp"
                    placeholder="Загрузите работы из диска С:/"
                    prepend-icon="mdi-camera"
                    label="Изображение"
                ></v-file-input>
            </v-form>

            <v-row v-if="isImageCollectionEmpty">
                <v-col 
                v-for="n in 9"
                :key="n"
                class="d-flex child-flex"
                cols="4"
                >
                <v-img
                    :src="`https://picsum.photos/500/300?image=${n * 5 + 10}`"
                    :lazy-src="`https://picsum.photos/10/6?image=${n * 5 + 10}`"
                    aspect-ratio="1"
                    class="grey lighten-2"
                >
                    <template v-slot:placeholder>
                    <v-row
                        class="fill-height ma-0"
                        align="center"
                        justify="center"
                    >
                        <v-progress-circular
                        indeterminate
                        color="grey lighten-5"
                        ></v-progress-circular>
                    </v-row>
                    </template>
                </v-img>
                </v-col>
            </v-row>
            <v-row v-else>
                <v-col 
                v-for="image in imageCollection"
                :key="image"
                class="d-flex child-flex"
                cols="4"
                >
                <v-img
                    :src="`file:///C:/${image}`"
                    :lazy-src="`file:///C:/${image}`"
                    aspect-ratio="1"
                    class="grey lighten-2"
                >
                    <template v-slot:placeholder>
                    <v-row
                        class="fill-height ma-0"
                        align="center"
                        justify="center"
                    >
                        <v-progress-circular
                        indeterminate
                        color="grey lighten-5"
                        ></v-progress-circular>
                    </v-row>
                    </template>
                </v-img>
                </v-col>
            </v-row>
        </v-container>
    </div>
</template>

<script>
import { mainStorePath } from "@/store";
import { mapState, mapActions } from "vuex";
import axios from "axios"

export default ({
    methods: {
        ...mapActions(mainStorePath, [
            
        ]),
        fileUploaded(files) {
            files.forEach(async ifile => {
                await axios.post('http://localhost:5000/api/upload', {file:ifile})
                .then(res => {
                    console.log(res)
                })
                .catch(err => {
                    console.error(err); 
                })
            });
        },
    },
    computed: {
        ...mapState(mainStorePath, [

        ]),
        isImageCollectionEmpty() {
            return this.imageCollection.length == 0;
        },
    },
    data() {
        return { 
            imageCollection: []
        }
    },
})
</script>
