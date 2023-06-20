<template>
    <div>
        <h1>Страница с постами</h1>
        <input-component
            v-model="searchQuery"
            placeholder="Поиск..."
            v-focus
        />
        <div class="app__btns">
            <button-component
            >
                Создать пост
            </button-component>
            <select-component
                v-model="selectedSort"
                :options="sortOptions"
            >
            </select-component>
        </div>
        <dialog-component 
            :show="dialogVisible"
            @update:show="dialogVisible = $event"
        >
            <post-form 
            >
            </post-form>
        </dialog-component>
        <post-list 
            :posts="sortedAndSearchedPosts"
            v-if="!isPostsLoading"
        >
        </post-list>
        <div v-else>Идет загрузка...</div>
    </div>
</template>


<script>
import PostForm from '@/components/PostForm.vue'
import PostList from '@/components/PostList.vue'
import ButtonComponent from '@/components/UI/ButtonComponent.vue'
import axios from 'axios';
import SelectComponent from '@/components/UI/SelectComponent.vue'
import {ref} from 'vue';
import imputComponent from '@/components/UI/InputComponent.vue';
import {usePosts} from "@/hooks/usePosts";
import useSortedPosts from "@/hooks/useSortedPosts";
import useSortedAndSearchedPosts from "@/hooks/useSortedAndSearchedPosts";

export default {
    components: {
        PostForm, PostList,
        ButtonComponent,
        SelectComponent
    },
    data() {
        return {
            dialogVisible: false,
            sortOptions: [
                {value: 'title', name: 'По названию'},
                {value: 'body', name: 'По описанию'},
            ]

        }
    },
    setup(props) {
        const {posts, totalPages, isPostsLoading} = usePosts(10)
        const {sortedPosts, selectedSort} = useSortedPosts(posts)
        const {searchQuery, sortedAndSearchedPosts} = useSortedAndSearchedPosts(sortedPosts)
        return {
            posts,
            totalPages,
            isPostsLoading,
            sortedPosts,
            selectedSort,
            searchQuery,
            sortedAndSearchedPosts
        }
    }
}
</script>


<style>
.app__btns{
    margin: 15px 0px;
    display: flex;
    justify-content: space-between;
}
.page__wrapper {
    display: flex;
    margin-top: 15px;
}
.page {
    border: 1px solid black;
    padding: 10px;
}
.current-page {
    border: 4px solid teal;
}
.observer {
    height: 30px;
    background: green;
}


</style>