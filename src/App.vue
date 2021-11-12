<template>
  <div class="home bg-white">
    <img alt="Vue logo" src="./assets/logo.png">
    <Login :method="irRegister" v-if="step=== 1"/>
    <Register :method="irLogin" v-if="step ===2" />
  </div>
</template>

<script>
import Login from '@/components/forms/Login';
import Register from '@/components/forms/Register';

export default {
  name: 'App',
  components: {
    Login,
    Register
  },
  methods:{
    irRegister(){
      this.step++;
    },
    irLogin(){
      this.step--
    }
  },
  props:{
    stepForm:{
      type:Number,
      default:1
    },
    methodNext_:{
      type: Function  
    },
    methodBack_:{
      type: Function  
    }
  },
  data() {
    return {
      step: this.stepForm,
      methodNext: this.methodNext_,
      methodBack: this.methodBack_,
    }
  }
}
</script>
  
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0;
  padding:0;
  box-sizing: border-box;
}
.home{
  height:100vh;
  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;
}
.home img{
  max-width:150px;
}
</style>
