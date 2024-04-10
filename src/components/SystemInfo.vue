<script setup>
import {ref} from "vue";
import {getSystemInfo, isValidSystemSetup} from "crypto-pro";

const systemInfo = ref();
const systemInfoError = ref();

isValidSystemSetup()
    .then((res) => getSystemInfo()
        .then((res2) => {
          systemInfo.value = {...res2, isValidSystemSetup: res}
        })
        .catch((error) => {
          systemInfoError.value = error.message
        })
    )
    .catch((error) => {
      systemInfoError.value = error.message
    })
</script>

<template>
    <pre>{{
        systemInfo ? (
            JSON.stringify(systemInfo, null, ' ')
        ) : (
            systemInfoError || null
        )
      }}</pre>
</template>

<style scoped>

</style>
