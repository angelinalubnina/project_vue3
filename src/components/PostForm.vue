<template>
    <form @submit.prevent>
        <h3>Создание поста</h3>
        <input-component 
            v-model="post.title"
            type="text" 
            placeholder="Название"
        ></input-component>
        <input-component 
            v-model="post.body"
            type="text" 
            placeholder="Описание"
        ></input-component>
        <button-component 
            @click="createPost"
            style="align-self: flex-end; margin-top: 15px;"
        >
            Создать
        </button-component>
    </form>
</template>

<script>
import InputComponent from './UI/InputComponent.vue'
    export default {
        components: { InputComponent },
        data() {
            return {
                post: {
                    title: '',
                    body: '',
                }
            }
        },
        methods: {
            createPost() {
                this.post.id = Date.now()
                this.$emit('create', this.post)
                this.post = {
                    title: '',
                    body: '',
                }
            },
        },
    }
</script>

<style scoped>
form {
    display: flex;
    flex-direction: column;
}
</style>