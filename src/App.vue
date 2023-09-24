<template>
  <div id="app">
    <otp-input></otp-input>
  </div>
</template>

<script>

import OtpInput from './components/otp.vue'
export default {
  name: 'App',
  components: {
  OtpInput
  },
  data(){
    return {
      otpCode: '',
    }
    
  },
  methods: {

  }

}
</script>

<style>

</style>
