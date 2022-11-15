<template>
    <div class="app">
        <PostFormVue
            @create="createPost"
        />
        <PostListVue 
            :posts="posts"
        />
    </div>
</template>

<script>

import PostFormVue from './components/PostForm.vue'
import PostListVue from './components/PostList.vue'

export default {
    components: {
        PostListVue, PostFormVue
    },
    data () {
        return {
            posts: [
                {id:1, title:"JavaScript", body:"Описание поста"},
                {id:2, title:"JavaScript 2", body:"Описание поста 2"},
                {id:3, title:"JavaScript 3", body:"Описание поста 3"},
            ],
        }
    },
    methods: {
        createPost(post) {
            this.posts.push(post)
        },
    }
}
</script>

<style>
    * {
        margin: 0;
        padding:0;
        box-sizing: border-box;
        font-family: Arial, Helvetica, sans-serif;
    }
    
    .app {
        padding: 20px;
    }
</style>