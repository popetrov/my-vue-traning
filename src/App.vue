<template>
    <div class="app">
        <h1>
            Страница с постами
        </h1>
        <MyButton class="btn_create" @click="showDialog">
            Создать пост
        </MyButton>
        <MyDialog v-model:show="dialogVisible">
            <PostFormVue
                @create="createPost"
            />
        </MyDialog>
        <PostListVue 
            :posts="posts"
            @remove="removePost"
            v-if="!isPostLoading"
        />
        <div v-else>Идет загрузка...</div>
    </div>
</template>

<script>

import PostFormVue from './components/PostForm.vue'
import PostListVue from './components/PostList.vue'
import MyButton from './components/UI/MyButton.vue'
import MyDialog from './components/UI/MyDialog.vue'
import axios from "axios"

export default {
    components: {
    PostListVue,
    PostFormVue,
    MyDialog,
    MyButton
},
    data () {
        return {
            posts: [],
            dialogVisible: false,
            isPostLoading: false
        }
    },
    methods: {
        createPost(post) {
            this.posts.push(post)
            this.dialogVisible = false
        },
        removePost(post) {
            this.posts = this.posts.filter(i => i.id !== post.id)
        },
        showDialog () {
        this.dialogVisible = true
        },
        async fetchPosts() {
            try {
                this.isPostLoading = true
                const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10')
                this.posts = response.data
            }catch(error){
                console.log(error)
            } finally {
                this.isPostLoading = false
            }
        },
    },
    mounted() {
            this.fetchPosts()
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

    .btn_create {
        margin-top: 10px;
    }
</style>