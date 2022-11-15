<template>
        <form @submit.prevent>
            <h4>Создание поста</h4>
            <MyInput 
                v-model="post.title" 
                type="text" 
                placeholder="Название"
            />
            <MyInput 
                v-model="post.body"
                type="text" 
                placeholder="Описание"
            />
            <MyButton 
                class="btn_create"
                @click="createPost" 
            >
                Опубликовать пост
            </MyButton>
        </form>
</template>

<script>
import MyInput from './UI/MyInput.vue'



    export default {
    data() {
        return {
            post: {
                title: "",
                body: ""
            }
        };
    },
    methods: {
        createPost() {
            this.post.id = Date.now();
            this.$emit("create", this.post);
            this.post = {
                title: "",
                body: ""
            };
        }
    },
    components: { MyInput }
}
</script>

<style scoped>
    form {
        display: flex;
        flex-direction: column;
    }

    .btn_create {
        margin-top: 10px;
    }

</style>