<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>

<script>
    export default {
        name: 'button-component'
    }
</script>

<style scoped>
.btn {
    padding: 10px 15px;
    background: none;
    color: teal;
    font-size: 16px;
    border: 1px solid teal;
}
</style>