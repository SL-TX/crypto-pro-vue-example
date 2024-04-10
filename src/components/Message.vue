<script setup>
const message = defineModel()
</script>

<template>
  <legend>Создание подписи</legend>
  <label>Подписываемое сообщение: *</label>
  <br/>
  <textarea
      :value="message"
      autoFocus
      cols="80"
      placeholder="Введите сообщение"
      required
      rows="5"/>
</template>

<style scoped>

</style>
