<template>
    <select :value="modelValue" @change="changeOption">
        <option disabled value="">Выберите из списка</option>
        <option 
            v-for="option in options" 
            :key="option.value" 
            :value="option.value"
        >
            {{ option.name }}
        </option>
    </select>
</template>

<script>
    export default {
        name: 'select-component',
        props: {
            modelValue: {
                type: String
            },
            options: {
                type: Array,
                default: () => []
            }
        },
        methods: {
            changeOption(event) {
                // const selectedValue = event.target.value
                this.$emit('update:modelValue', event.target.value)
            }
        }
    }
</script>

<style scoped>

</style>