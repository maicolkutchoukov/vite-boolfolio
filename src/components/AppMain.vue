<script>
import axios from 'axios';

export default {
    data() {
        return {
            projects:[],
            currentPage: 1,
            lastPage:null
        };
    },
    methods: {
        getProjects(page){
            axios.get('http://127.0.0.1:8000/api/projects', {
                params:{
                    page: page
                }
                }) // URL DELL'API
            .then((response) => {
                this.projects = response.data.results.data
                console.log(this.projects)
                this.currentPage = response.data.results.current_page
                this.lastPage = response.data.results.last_page
            })
        },
        prevPage(){
            if (this.currentPage > 1) {
                this.getProjects(this.currentPage - 1)
            }
        },
        nextPage(){
            if (this.currentPage < this.lastPage){
                this.getProjects(this.currentPage + 1)
            }
        }
    },
    created(){
        this.getProjects(this.currentPage)
    }
}
</script>

<template>
    <main>
        <div class="container">
            <div class="row">
                <div class="col-3" v-for="project in projects" :key="project.id">
                    <div class="card" style="width: 18rem;">
                        <img :src="'http://127.0.0.1:8000/storage/'+project.cover_img" class="card-img-top" alt="...">
                        <div class="card-body">
                            <p class="card-text">
                                {{ project.title }}
                            </p>
                            <p>
                                {{ project.content }}
                            </p>
                        </div>
                    </div>
                </div>
            </div>
            <div>
                <button @click="prevPage()">
                    Prev
                </button>
                <button @click="nextPage()">
                    Next
                </button>
            </div>
        </div>
    </main>
</template>

<style lang="scss" scoped>
.card{
    min-height: 500px;
}
</style>
