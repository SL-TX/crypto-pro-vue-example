<script setup>
const signature = defineModel('signature')
const signatureStatus = defineModel('signatureStatus')
const signatureError = defineModel('signatureError')
</script>

<template>
    <label>Подпись (PKCS7):</label>
    <br/>
    <textarea
            id="signature"
            cols="80"
            rows="30"
            :value="signature"
            :placeholder="signatureStatus"
            readOnly/>
    <pre>{{ signatureError || null }}</pre>
</template>

<style scoped>

</style>
