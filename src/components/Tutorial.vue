<template>
    <div v-if="currentTutorial" class="edit-form">
        <h4>Tutorial/ {{id}}</h4>
        <div>
            <h2>Title</h2>
            {{currentTutorial.title}}
        </div>
        <div>
            <h2>Description</h2>
            {{currentTutorial.body}}
        </div>
        <p>{{ message }}</p>
    </div>

    <div v-else>
        <br/>
        <p>Please click on a Tutorial...</p>
    </div>
</template>

<script>
    import TutorialDataService from "../services/TutorialDataService";

    export default {
        name: "tutorial",
        data() {
            return {
                currentTutorial: null,
                message: '',
                id: ''
            };
        },
        methods: {
            getTutorial(id) {
                console.log(id);
                TutorialDataService.get(id)
                    .then(response => {
                        this.currentTutorial = response.data;
                        console.log(response.data);
                    })
                    .catch(e => {
                        console.log(e);
                    });
            },

            updatePublished(status) {
                var data = {
                    id: this.currentTutorial.id,
                    title: this.currentTutorial.title,
                    description: this.currentTutorial.description,
                    published: status
                };

                TutorialDataService.update(this.currentTutorial.id, data)
                    .then(response => {
                        this.currentTutorial.published = status;
                        console.log(response.data);
                    })
                    .catch(e => {
                        console.log(e);
                    });
            },

            updateTutorial() {
                TutorialDataService.update(this.currentTutorial.id, this.currentTutorial)
                    .then(response => {
                        console.log(response.data);
                        this.message = 'The tutorial was updated successfully!';
                    })
                    .catch(e => {
                        console.log(e);
                    });
            },

            deleteTutorial() {
                TutorialDataService.delete(this.currentTutorial.id)
                    .then(response => {
                        console.log(response.data);
                        this.$router.push({name: "tutorials"});
                    })
                    .catch(e => {
                        console.log(e);
                    });
            }
        },
        mounted() {
            this.message = '';
            this.id = this.$route.params.id;
            this.getTutorial(this.$route.params.id);
        }
    };
</script>

<style>
    .edit-form {
        max-width: 300px;
        margin: auto;
    }
</style>
