<template>
    <div class="app">
        <h1>
            Страница с постами
        </h1>
        <MyInput
            v-model="searchQuery"
            placeholder="Поиск..."
        />
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
            :posts="sortedAndSearchedPosts"
            @remove="removePost"
            v-if="!isPostLoading"
        />
        <div v-else>Идет загрузка...</div>
        <div class="page-wrapper">
            <div 
                v-for="pageNumber in totalPage"
                :key="pageNumber"
                class="page"
                :class="{
                    'current-page': page === pageNumber
                }"
                @click="changePage(pageNumber)"
            >
                {{pageNumber}}
            </div>
        </div>
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
            searchQuery:'',
            page: 1,
            limit: 10,
            totalPage: 0,
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
                const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                    params: {
                        _page: this.page,
                        _limit: this.limit
                    }
                })
                this.totalPage = Math.ceil(response.headers['x-total-count']/this.limit)
                this.posts = response.data
            }catch(error){
                console.log(error)
            } finally {
                this.isPostLoading = false
            }
        },
        changePage(pageNumber) {
            this.page = pageNumber
            this.fetchPosts()
        }
    },
    mounted() {
            this.fetchPosts()
        },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => (post1[this.selectedSort])?.localeCompare(post2[this.selectedSort]))
        },
        sortedAndSearchedPosts() {
            return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
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

    .page-wrapper {
        display: flex;
        margin-top: 15px;
        justify-content: center;
    }

    .page {
        margin-left: 5px;
        border: 1px solid black;
        padding: 10px;
        cursor: pointer;
    }

    .current-page {
        border: 2px solid teal;
        cursor: pointer;
    }
</style>