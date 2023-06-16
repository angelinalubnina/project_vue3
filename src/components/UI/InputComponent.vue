<template>
    <input :value="modelValue" @input="updateInput" class="inputPost" type="text">
</template>

<script>
    export default {
        name: 'input-component',
        props: {
            modelValue: [String, Number]
        },
        methods: {
            updateInput(event) {
                this.$emit('update:modelValue', event.target.value)
            }
        }
    }
</script>

<style scoped>
.inputPost {
    width: 100%;
    border: 2px solid teal;
    padding: 10px 15px;
    margin-top: 10px;
}
</style>