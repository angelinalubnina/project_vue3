<template>
    <div>
        <h1>Страница с постами</h1>
        <input-component
            :model-value="searchQuery"
            @update:model-value="setSearchQuery"
            placeholder="Поиск..."
            v-focus
        />
        <div class="app__btns">
            <button-component
                @click="showDialog"
            >
                Создать пост
            </button-component>
            <select-component
                :model-value="selectedSort"
                @update:model-value="setSelectedSort"
                :options="sortOptions"
            >
            </select-component>
        </div>
        <dialog-component 
            :show="dialogVisible"
            @update:show="dialogVisible = $event"
        >
            <post-form 
                @create="createPost"
            >
            </post-form>
        </dialog-component>
        <post-list 
            :posts="sortedAndSearchedPosts"
            @remove="removePost"
            v-if="!isPostsLoading"
        >
        </post-list>
        <div v-else>Идет загрузка...</div>
        <div v-intersection="loadMorePosts" class="observer"></div>

        <!-- <div class="page__wrapper">
            <div 
                v-for="pageNumber in totalPages" 
                :key="pageNumber"
                class="page"
                :class="{
                    'current-page': page === pageNumber
                }"
                @click="changePage(pageNumber)"
            >
                {{ pageNumber }}
            </div>
        </div> -->
    </div>
</template>


<script>
import PostForm from '@/components/PostForm.vue'
import PostList from '@/components/PostList.vue'
import ButtonComponent from '@/components/UI/ButtonComponent.vue'
import axios from 'axios';
import SelectComponent from '@/components/UI/SelectComponent.vue'
import {mapState, mapGetters, mapActions, mapMutations} from 'vuex';

export default {
    components: {
        PostForm, PostList,
        ButtonComponent,
        SelectComponent
    },
    data() {
        return {
            dialogVisible: false,
        }
    },
    // прочитать про Proxy
    methods: {
        ...mapMutations({
            setPage: 'post/setPage',
            setSearchQuery: 'post/setSearchQuery',
            setSelectedSort: 'post/setSelectedSort'
        }),
        ...mapActions({
            loadMorePosts: 'post/loadMorePosts',
            fetchPosts: 'post/fetchPosts'
        }),
        createPost(post) { 
            this.posts.push(post);
            this.dialogVisible = false
        },
        removePost(post) {
            this.posts = this.posts.filter(p => p.id !== post.id)
        },
        showDialog() {
            this.dialogVisible = true
        },
    },
    mounted() {
        this.fetchPosts();
    },
    computed: {
        ...mapState({
            posts: state => state.post.posts,
            isPostsLoading: state => state.post.isPostsLoading,
            selectedSort: state => state.post.selectedSort,
            searchQuery: state => state.post.searchQuery,
            page:  state => state.post.page,
            limit: state => state.post.limit,
            totalPages: state => state.post.totalPages,
            sortOptions: state => state.post.sortOptions,
        }),
        ...mapGetters({
            sortedPosts: 'post/sortedPosts',
            sortedAndSearchedPosts: 'post/sortedAndSearchedPosts'
        })
    },
    watch: {
        // page() {
        //     this.fetchPosts()
        // }
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
