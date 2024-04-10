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
      :placeholder="hashStatus"
      :value="hash"
      cols="80"
      readOnly
      rows="5"/>
  <pre>{{ hashError || null }}</pre>
</template>

<style scoped>

</style>
