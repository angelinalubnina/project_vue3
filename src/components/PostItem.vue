<template>
    <div>
        <div class="post">
            <div>
                <div>{{ post.id }}</div>
                <div><strong>Название</strong> {{ post.title }}</div>
                <div><strong>Описание</strong> {{ post.body }}</div>
            </div>
            <div class="post__btns">
                <button-component
                    @click="$router.push(`/posts/${post.id}`)"
                >
                    Открыть
                </button-component>
                <button-component
                    @click="$emit('remove', post)"
                >
                    Удалить
                </button-component>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            post: {
                type: Object,
                required: true
            }
        }
    }
</script>

<style scoped>
.post {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 15px;
    border: 2px solid teal;
    margin-top: 15px;

}
.post__btns{
    display: flex;
}
</style>