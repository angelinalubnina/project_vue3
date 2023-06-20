<template>
    <div class="navbar">
        <div style="font-size: 30px;" @click="$router.push('/')">Vue 3</div>
        <div class="navbar__btns">
            <button-component @click="$router.push('/posts')">Посты</button-component>
            <button-component style="margin-left: 20px;" @click="$router.push('/about')">О приложении</button-component>
            <button-component style="margin-left: 20px;" @click="$router.push('/store')">store</button-component>
            <button-component style="margin-left: 20px;" @click="$router.push('/composition')">composition</button-component>
        </div>
    </div>
</template>

<script>
    export default {

        
    }
</script>

<style>
.navbar {
    height: 55px;
    background-color: lightgray;
    box-shadow: 2px 2px 4px gray;
    display: flex;
    align-items: center;
    padding: 0 15px;
}
.navbar__btns {
    margin-left: auto;
}
</style>