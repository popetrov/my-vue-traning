<template>
    <div v-if="posts.length > 0">
        <h3>Список постов</h3>
        <transition-group>
            <PostItem
                v-for="post in posts"
                :post="post" 
                :key="post.id"
                @remove="$emit('remove', post)"
            />
        </transition-group>
    </div>
    <h2 v-else class="no_post">
        Постов нет
    </h2>
</template>

<script>
import PostItem from './PostItem.vue';

    export default {
    props: {
        posts: {
            type: Array,
            required: true,
        }
    },
    components: { PostItem }
}
</script>

<style scoped> 
    .no_post {
        color: red;
    }
</style>