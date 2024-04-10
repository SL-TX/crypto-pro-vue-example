<script setup>
 const message = defineModel()
</script>

<template>
    <legend>Создание подписи</legend>
    <label>Подписываемое сообщение: *</label>
    <br/>
    <textarea
            cols="80"
            rows="5"
            placeholder="Введите сообщение"
            :value="message"
            autoFocus
            required/>
</template>

<style scoped>

</style>
