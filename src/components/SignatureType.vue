<script setup>
const type = defineModel()
</script>

<template>
    <label>Тип подписи: *</label>
    <br/>
    <label>
        <input
                type="radio"
                :value="false"
                v-model="type"
                >Совмещенная</label>
    <br/>
    <label>
        <input
                type="radio"
                :value="true"
                v-model="type"
                >Отделенная</label>
</template>

<style scoped>

</style>
