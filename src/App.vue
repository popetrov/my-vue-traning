<template>
    <div class="app">
        <h1>
            Страница с постами
        </h1>
        <div class="app_btns">
            <MyButton 
                class="btn_create" 
                @click="showDialog"
            >
                Создать пост
            </MyButton>
            <MySelect
                v-model="selectedSort"
                :options="sortOptions"
            />
        </div>
        <MyDialog v-model:show="dialogVisible">
            <PostFormVue
                @create="createPost"
            />
        </MyDialog>
        <PostListVue 
            :posts="sortedPosts"
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
import MySelect from './components/UI/MySelect.vue'
import axios from "axios"

export default {
    components: {
    PostListVue,
    PostFormVue,
    MyDialog,
    MyButton,
    MySelect
},
    data () {
        return {
            posts: [],
            dialogVisible: false,
            isPostLoading: false,
            selectedSort: '',
            sortOptions: [
                {value: 'title', name: 'По названию'},
                {value: 'body', name: 'По содержимому'}
            ]
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
        },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => (post1[this.selectedSort])?.localeCompare(post2[this.selectedSort]))
        }
    },
    watch: {

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


    .app_btns {
        margin: 10px 0;
        display: flex;
        justify-content: space-between;
    }
</style>