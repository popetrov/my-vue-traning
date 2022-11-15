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
        />
    </div>
</template>

<script>

import PostFormVue from './components/PostForm.vue'
import PostListVue from './components/PostList.vue'
import MyButton from './components/UI/MyButton.vue'
import MyDialog from './components/UI/MyDialog.vue'

export default {
    components: {
    PostListVue,
    PostFormVue,
    MyDialog,
    MyButton
},
    data () {
        return {
            posts: [
                {id:1, title:"JavaScript", body:"Описание поста"},
                {id:2, title:"JavaScript 2", body:"Описание поста 2"},
                {id:3, title:"JavaScript 3", body:"Описание поста 3"},
            ],
            dialogVisible: false,
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
    }
    },
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