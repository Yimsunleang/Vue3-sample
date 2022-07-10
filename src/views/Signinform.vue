<template>
  <form @submit.prevent="handleSubmit">
    <label>Email*:</label>
    <input type="text" v-model="state.email" />
    <div class="emailclass">
      <span class="error" v-if="v$.email.$error">{{v$.email.$errors[0].$message}}</span>
    </div>
    <label>Password*:</label>
    <input type="password" v-model="state.password" />
    <!-- <div v-if="showError" class= "error">{{ showError }} </div> -->
    <span class="error" v-if="v$.password.$error">{{v$.password.$errors[0].$message}}</span>
    <span class="error">{{ error }}</span>
    <div class="submit">
      <button @click="submitForm">Submit</button>
    </div>
  </form>
</template>
<script>
import router from "@/router";
import useValidate from "@vuelidate/core";
import { required, email, minLength } from "@vuelidate/validators";
import { reactive, computed, ref, watch } from "vue";
import HomeView from "../views/HomeView.vue";
export default {
  // component:{HomeView},
  setup() {
    const state = reactive({
      email: "",
      password: ""
    });
    const rules = computed(() => {
      return {
        email: { required, email },
        password: { required, minLength: minLength(5) }
      };
    });
    const error = ref("");
    const v$ = useValidate(rules, state);
    function submitForm() {
      this.v$.$validate();
      if(!this.v$.$error){
           if (
        state.email == "yimsunleang@gmail.com" &&
        state.password == "123456789"
      ) {
        router.push("/");
      } else {
        error.value = "Wrong email or password";
      }
      }
    //   if (
    //     state.email == "yimsunleang@gmail.com" &&
    //     state.password == "123456789"
    //   ) {
    //     router.push("/");
    //   } else {
    //     error.value = "Wrong email or password";
    //   }
      // if(!this.v$.$error){
      //     alert('login')
      // }
      // else{
      //     this.error.value = 'Login failed'
      // }
      // console.log(this.state)
    }
    watch(state, () => {
      error.value = "";
    });
    return {
      state,
      v$,
      error,
      submitForm
    };
  }
  // methods:{
  //     submitForm(){
  //         this.v$.$validate()
  //         if(this.state.email=='yimsunleang@gmail.com' && this.state.password=='123456789'){
  //             this.$router.push('/');
  //         }
  //         else{
  //             this.error.value = 'Wrong email or password'
  //         }

  // if(!this.v$.$error){
  //     alert('login')
  // }
  // else{
  //     this.error.value = 'Login failed'
  // }
  // console.log(this.state)
  // }
  // }
};
</script>
<style>
form {
  max-width: 420px;
  margin: 30px auto;
  background: white;
  text-align: left;
  padding: 40px;
  border-radius: 10px;
  box-shadow: 2px 5px rgb(231, 230, 230);
}
label {
  color: #aaa;
  display: inline-block;
  margin: 25px 0 15px;
  font-size: 0.8em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}
input {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  color: #555;
}
button {
  background: #0b6dff;
  border: 0;
  padding: 10px 30px;
  margin-top: 20px;
  color: white;
  border-radius: 20px;
}
.submit {
  text-align: center;
}
.error {
  color: red;
  margin-top: 10px;
  font-size: 0.8em;
  font-weight: bold;
}
.emailclass {
  display: block;
}
</style>