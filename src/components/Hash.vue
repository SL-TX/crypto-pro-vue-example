<script setup>
const hash = defineModel('hash')
const hashStatus = defineModel('hashStatus')
const hashError = defineModel('hashError')
</script>

<template>
    <legend>Результат</legend>
    <label>Хеш (ГОСТ Р 34.11-2012 256 бит):</label>
    <br/>
    <textarea
            id="hash"
            cols="80"
            rows="5"
            :value="hash"
            :placeholder="hashStatus"
            readOnly/>
    <pre>{{ hashError || null }}</pre>
</template>

<style scoped>

</style>
