<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>Data Post</h4>
                        <hr>
                        <router-link :to="{name:'post.create'}" class="btn btn-md btn-success">Tambah Post</router-link>
                        <table class="table table-striped table-bordered mt-4">
                            <thead class="thead-dark">
                                <tr class="text-center">
                                    <th scope="col">Title</th>
                                    <th scope="col">Content</th>
                                    <th scope="col">Options</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(post, index) in posts" :key="index">
                                    <td>{{post.title}}</td>
                                    <td>{{post.content}}</td>
                                    <td class="text-center">
                                         <router-link :to="{name: 'post.edit', params:{id: post.id }}" class="btn btn-sm btn-primary mr-1">EDIT</router-link>
                                        <button @click.prevent="postDelete(post.id)" class="btn btn-sm btn-danger shadow m-1">Hapus</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>    
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { onMounted, ref } from 'vue'
export default {
    setup(){
        // rective state
        let posts = ref([])

        // mounted
        onMounted(() => {
            // get Api from laravel Backend
            axios.get('http://localhost:8000/api/post')
            .then(response => {
                // assign state posts with response data
                posts.value = response.data.data
            }).catch(error=> {
                console.log(error.response.data)
            })
        })


        function postDelete(id){
            axios.delete(`http://localhost/api/post/${id}`)
            .then(() => {
                // splice posts
                const index = this.posts.findIndex(post => post.id === id) // find the post index
                if (~index) {
                // if the post exists in array
                this.posts.splice(index, 1)
                }
                // posts.value.splice(posts.value.indexOf(id),1);
            }).catch(error => {
                console.log(error.response.data)
            })
        }

        return {
            posts,
            postDelete
        }
    }
}
</script>

<style>
    body{
        background: lightgray;
    }
</style>