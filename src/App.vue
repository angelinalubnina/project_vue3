<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <input-component
            v-model="searchQuery"
            placeholder="Поиск..."
        />
        <div class="app__btns">
            <button-component
                @click="showDialog"
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
                @create="createPost"
            >
            </post-form>
        </dialog-component>
        <post-list 
            :posts="sortedAndSearchedPost"
            @remove="removePost"
            v-if="!isPostsLoading"
        >
        </post-list>
        <div v-else>Идет загрузка...</div>
        <div class="page__wrapper">
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
        </div>
    </div>
</template>


<script>
import PostForm from './components/PostForm.vue'
import PostList from './components/PostList.vue'
import ButtonComponent from './components/UI/ButtonComponent.vue'
import axios from 'axios';
import SelectComponent from './components/UI/SelectComponent.vue'

export default {
    components: {
        PostForm, PostList,
        ButtonComponent,
        SelectComponent
    },
    data() {
        return {
            posts: [],
            dialogVisible: false,
            isPostsLoading: false,
            selectedSort: '',
            searchQuery: '',
            page: 1,
            limit: 10,
            totalPages: 0,
            sortOptions: [
                {value: 'title', name: 'По названию'},
                {value: 'body', name: 'По описанию'},
            ]

        }
    },
    // прочитать про Proxy
    methods: {
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
        changePage(pageNumber) {
            this.page = pageNumber
        },
        async fetchPosts() {
            try {
                this.isPostsLoading = true
                const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                    params: {
                        _page: this.page,
                        _limit: this.limit
                    }
                })
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
                this.posts = response.data

            } catch(e) {
                console.log('Ошибка')
            } finally {
                this.isPostsLoading = false
            }
        }
    },
    mounted() {
        this.fetchPosts();
    },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
        },
        sortedAndSearchedPost() {
            return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
        }
    },
    watch: {
        page() {
            this.fetchPosts()
        }
    }
}
</script>


<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.app {
    padding: 20px;
}
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


</style>
