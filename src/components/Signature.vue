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
      :placeholder="signatureStatus"
      :value="signature"
      cols="80"
      readOnly
      rows="30"/>
  <pre>{{ signatureError || null }}</pre>
</template>

<style scoped>

</style>
