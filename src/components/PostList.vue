<template>
    <!-- <div> -->
    <div v-if="posts.length > 0">
        <h3>Список постов</h3>
        <transition-group name="post-list">
            <post-item 
                v-for="post in posts" 
                :key="post.id" 
                :post="post"
                @click="$router.push(`/posts/${post.id}`)"
                @remove="$emit('remove', post)"
            >
            </post-item>
        </transition-group>
    </div>
    <h2 v-else style="color: red">
        Список постов пуст 
    </h2>
</template>

<script>
import PostItem from './PostItem.vue'
    export default {
        components: { PostItem },
        props: {
            posts: {
                type: Array,
                required: true,
            }
        }
    }
</script>
 
<style scoped>
.post-list-enter-active,
.post-list-leave-active {
  transition: all 0.5s ease;
}
.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
.post-list-move, /* apply transition to moving elements */
.post-list-enter-active,
.post-list-leave-active {
  transition: all 0.5s ease;
}
</style>