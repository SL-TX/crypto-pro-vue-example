<script setup>
const type = defineModel()
</script>

<template>
  <label>Тип подписи: *</label>
  <br/>
  <label>
    <input
        v-model="type"
        :value="false"
        type="radio"
    >Совмещенная</label>
  <br/>
  <label>
    <input
        v-model="type"
        :value="true"
        type="radio"
    >Отделенная</label>
</template>

<style scoped>

</style>
