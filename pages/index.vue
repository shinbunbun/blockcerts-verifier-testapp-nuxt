
<template>
  <div class="container">
    <div>
      <h1 class="title">
        blockcerts-verifier-testapp-nuxt
      </h1>
    </div>
  </div>
</template>

<script>
/* eslint-disable no-console */
/* eslint-disable new-cap */
/* eslint-disable no-unused-vars */
// eslint-disable-next-line no-unused-vars
// import { Certificate } from '@blockcerts/cert-verifier-js'
export default {
  // eslint-disable-next-line require-await
  async mounted () {
    console.log(1)
    // eslint-disable-next-line import/no-absolute-path
    const data = require('/Users/yuto/Desktop/lastrust/59fd7320-3132-4213-b406-5dcb64ef55f1.json')
    // eslint-disable-next-line import/no-absolute-path
    // const data = require('/Users/yuto/Downloads/005b9d00-2cb3-458c-9bc0-48396932a058.json')
    const options = {
      locale: 'ja',
      serviceName: 'etherscan',
      key: process.env.API_KEY,
      keyPropertyName: 'apiKey'
      /* serviceURL: {
        main: 'https://api.etherscan.io/api?module=proxy',
        test: 'https://api-ropsten.etherscan.io/api?module=proxy'
      } */
    }
    // eslint-disable-next-line no-undef
    const certificate = new Verifier.Certificate(data, options)
    await certificate.init()
    // console.log(await certificate.parseJson(data))
    const verificationResult = await certificate.verify(({ code, label, status, errorMessage }) => {
      console.log('Code:', code, label, ' - Status:', status)
      if (errorMessage) {
        console.log(`The step ${code} fails with the error: ${errorMessage}`)
      }
    })

    console.log(verificationResult)

    if (verificationResult.status === 'failure') {
      console.log(`The certificate is not valid. Error: ${verificationResult.errorMessage}`)
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
