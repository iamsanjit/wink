<script type="text/ecmascript-6">

    export default {

        components: {
        },

        /**
         * The component's data.
         */
        data() {
            return {
                uploading: false
            };
        },


        /**
         * Prepare the component.
         */
        mounted() {
            
        },


        methods: {
            /**
             * Upload the selected feed file.
             */
            uploadSelectedFeed(event){
                let file = event.target.files[0];
                let formData = new FormData();

                formData.append('feed', file, file.name);

                this.uploading = true;

                this.http().post('/api/imports/wordpress', formData, {
                    onUploadProgress: progressEvent => {
                        this.uploadProgress = Math.round((progressEvent.loaded * 100) / progressEvent.total);
                    }
                }).then(response => {
                    this.uploading = false;
                }).catch(error => {
                });
            },
        }
    }
</script>

<template>
    <div>
        <page-header>
            <template slot="right-side">
                <!-- <router-link :to="{name:'post-new'}" class="py-1 px-2 btn-primary text-sm">
                    Settings
                </router-link> -->
            </template>
        </page-header>

        <div class="container">

            <h2 class="font-semibold text-3xl mb-10">
                Settings
            </h2>
            
            <h2 class="font-semibold text-xl mb-10">
                Import Wordpress Feed
            </h2>

            <div v-if="uploading">
                <preloader></preloader>
            </div>

            <div class="flex items-center" v-if="!uploading">
                <input type="file" class="hidden" id="feed" accept="text/xml" v-on:change="uploadSelectedFeed">

                <label for="feed" class="cursor-pointer underline">Upload Feed</label>
            </div>
        </div>
    </div>
</template>
