<template>
    <div class="app">
        <form @submit.prevent>
            <h4>Создание поста</h4>
            <input 
                v-bind:value="title"
                @input="title = $event.target.value" 
                class="input" type="text" 
                placeholder="Название"
            >
            <input 
                v-bind:value="body"
                @input="body = $event.target.value" 
                class="input" 
                type="text" 
                placeholder="Описание"
            >
            <button 
                class="btn" 
                @click="createPost"
            >
                Опубликовать пост
            </button>
        </form>
        <div class="post" v-for="post in posts">
            <div><strong>Название:</strong>{{post.title}}</div>
            <div><strong>Описание:</strong>{{post.body}}</div>
        </div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            posts: [
                {id:1, title:"JavaScript", body:"Описание поста"},
                {id:2, title:"JavaScript 2", body:"Описание поста 2"},
                {id:3, title:"JavaScript 3", body:"Описание поста 3"},
            ],
            title:'',
            body:'',
        }
    },
    methods: {
        createPost() {
            const newPost = {
                id: Date.now(),
                title: this.title,
                body: this.body
            }
            this.posts.push(newPost)
            this.title = ''
            this.body = ''
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

    .post {
        margin-top: 15px;
        padding: 15px;
        border: 2px solid teal;
    }

    .input {
        width: 100%;
        border: 1px solid teal;
        padding: 10px 15px;
        margin-top: 10px;
    }

    .app {
        padding: 20px;
    }

    form {
        display: flex;
        flex-direction: column;
    }

    .btn {
        margin-top: 10px;
        align-self: flex-end;
        padding: 10px 15px;
        background: none;
        color: teal;
        border: 1px solid teal;
    }
</style>