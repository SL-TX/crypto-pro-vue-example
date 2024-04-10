<script setup>
import {getCertificate, getUserCertificates} from 'crypto-pro';
import {ref} from "vue";

const cert = defineModel();
const certs = ref([]);
const certserr = ref([]);
const certDetails = ref(null)
const certDetailErr = ref([]);
getUserCertificates()
    .then(
        (res) => {
          certs.value = res
        }
    ).catch((error) => {
  certserr.value = error.message;
})

const changeCert = (event) => {
  cert.value = certs.value.find(({thumbprint}) => thumbprint === event.target.value)
}
const loadCertificateDetails = async (thumbprint) => {

  try {
    const certificate = await getCertificate(thumbprint);
    certDetails.value = {
      name: certificate.name,
      issuerName: certificate.issuerName,
      subjectName: certificate.subjectName,
      thumbprint: certificate.thumbprint,
      validFrom: certificate.validFrom,
      validTo: certificate.validTo,
      isValid: await certificate.isValid(),
      version: await certificate.getCadesProp('Version'),
      base64: await certificate.exportBase64(),
      algorithm: await certificate.getAlgorithm(),
      extendedKeyUsage: await certificate.getExtendedKeyUsage(),
      ownerInfo: await certificate.getOwnerInfo(),
      issuerInfo: await certificate.getIssuerInfo(),
      decodedExtendedKeyUsage: await certificate.getDecodedExtendedKeyUsage(),
      '1.3.6.1.4.1.311.80.1': await certificate.hasExtendedKeyUsage('1.3.6.1.4.1.311.80.1'),
      '[\'1.3.6.1.5.5.7.3.2\', \'1.3.6.1.4.1.311.10.3.12\']': await certificate.hasExtendedKeyUsage([
        '1.3.6.1.5.5.7.3.2',
        '1.3.6.1.4.1.311.10.3.12'
      ]),
      '1.3.6.1.4.1.311.80.2': await certificate.hasExtendedKeyUsage('1.3.6.1.4.1.311.80.2'),
      '\'1.3.6.1.5.5.7.3.3\', \'1.3.6.1.4.1.311.10.3.12\'': await certificate.hasExtendedKeyUsage([
        '1.3.6.1.5.5.7.3.3',
        '1.3.6.1.4.1.311.10.3.12'
      ]),
    }
  } catch (error) {
    certDetailErr.value = error;
  }
}

</script>

<template>
  <label for="certificate">Сертификат: *</label>
  <br>
  <select id="certificate" @change="changeCert">
    <option>Не выбран</option>
    <option v-for="c in certs" :key="c.thumbprint" :value="c.thumbprint">{{ c.name }} (действителен до:
      {{ c.validTo }})
    </option>
  </select>
  <pre>
      <template v-for="c in certserr">{{ c }}</template>
  </pre>
  <details v-if="cert" @click="loadCertificateDetails(cert)">
    <summary>Информация о сертификате</summary>
    <pre>
        {{
        certDetails ? (
            JSON.stringify(certDetails, null, '  ')
        ) : 'Запрашивается...'
      }}
    </pre>
  </details>
  <pre>
      <template v-for="c in certDetailErr">{{ c }}</template>
  </pre>
</template>

<style scoped>

</style>
