<template>
  <div id="app">
    <otp-input @otp-complete="handleCode"></otp-input>
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
handleCode(otpCode){
  this.otpCode = otpCode
  console.log(otpCode)
  }
  }
}
</script>

<style>

</style>
