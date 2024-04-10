<script lang="ts" setup>

import Message from "./components/Message.vue";
import {ref} from "vue";
import Certificate from "./components/Certificate.vue";
import SignatureType from "./components/SignatureType.vue";
import Hash from "./components/Hash.vue";
import Signature from "./components/Signature.vue";
import CustomSystemInfo from "./components/CustomSystemInfo.vue";
import SystemInfo from "./components/SystemInfo.vue";
import {createAttachedSignature, createDetachedSignature, createHash} from "crypto-pro";

const message = ref("Привет мир!");
const cert = ref(null);
const detachedSignature = ref(false);
const hash = ref('')
const hashStatus = ref('Не вычислен')
const hashError = ref(null)
const signature = ref('')
const signatureStatus = ref('Не создана')
const signatureError = ref(null)

const createSignature= async () => {
    signature.value = ''
    signatureError.value = null
    hash.value = ''
    hashError.value = null
    hashStatus.value = 'Вычисляется...'
    await createHash(message.value)
        .then((res)=> {
            hash.value = res
        })
        .catch((error)=> {
            hashError.value = error.message
        })
    hashStatus.value = 'Не вычислен'
    signatureStatus.value = 'Создается...'
    if (detachedSignature.value) {
        await createDetachedSignature(cert.value.thumbprint,hash.value)
            .then((res)=> {
                signature.value = res
            })
            .catch((error)=> {
                signatureError.value = error.message
            })
        signatureStatus.value = 'Не создана'
        return;
    }
    await createAttachedSignature(cert.value.thumbprint,message.value)
        .then((res)=> {
            signature.value = res
        })
        .catch((error)=> {
            signatureError.value = error.message
        })
    signatureStatus.value = 'Не создана'
}
</script>

<template>
  <form novalidate="" @submit.prevent="createSignature">
    <fieldset>
      <Message v-model="message"/>
      <br><br>
      <Certificate v-model="cert"></Certificate>
        {{detachedSignature}}
      <SignatureType v-model="detachedSignature"></SignatureType>
      <br><br>
      <hr>
      <button :disabled="!cert || !message " type="submit">Создать подпись</button>
    </fieldset>
  </form>
  <fieldset>
    <Hash
        :hash="hash"
        :hashError="hashError"
        :hashStatus="hashStatus"/>

    <Signature
        :signature="signature"
        :signatureError="signatureError"
        :signatureStatus="signatureStatus"/>
    <p>Для <a href="https://www.gosuslugi.ru/pgu/eds/" rel="nofollow noopener noreferrer" target="_blank"
              title="Перейти к проверке подписи">проверки</a> нужно создать файл со сгенерированной подписью в
      кодировке UTF-8 с расширением *.sgn<br>для отделенной подписи (или *.sig для совмещенной).</p></fieldset>

  <fieldset>
    <legend>Информация о системе</legend>
      <CustomSystemInfo/>
      <SystemInfo/>
  </fieldset>
</template>

<style scoped>

</style>
