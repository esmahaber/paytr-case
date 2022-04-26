<template>
  <Header/>
  <ReceiptForm/>
</template>

<script>

import ReceiptForm from './components/ReceiptForm.vue'
import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {
    ReceiptForm,
    Header
  }
}
</script>