<template>
  <div class="min-h-screen flex items-center justify-center py-12 px-4 sm:px-6 lg:px-8">
  <div class="max-w-md w-full">
    <div class="mb-6">
      <img class="mx-auto h-12 w-auto" src="https://tailwindui.com/img/logos/workflow-mark-on-white.svg" alt="Workflow">
      <h2 class="mt-6 text-center md:text-3xl text-2xl leading-9 font-extrabold text-gray-900">
        Sign in to your account
      </h2>
    </div>

      <!-- <div class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative" role="alert" v-if="error">
        <strong class="font-bold">{{ error}}</strong>
        <span class="absolute top-0 bottom-0 right-0 px-4 py-3">
          <svg class="fill-current h-6 w-6 text-red-500" role="button" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><title>Close</title><path d="M14.348 14.849a1.2 1.2 0 0 1-1.697 0L10 11.819l-2.651 3.029a1.2 1.2 0 1 1-1.697-1.697l2.758-3.15-2.759-3.152a1.2 1.2 0 1 1 1.697-1.697L10 8.183l2.651-3.031a1.2 1.2 0 1 1 1.697 1.697l-2.758 3.152 2.758 3.15a1.2 1.2 0 0 1 0 1.698z"/></svg>
        </span>
      </div> -->

    <div class="w-full max-w-xl p-6 text-left shadow bg-white rounded-lg">

      <div class="flex flex-wrap -mx-3 py-3">
          <div class="flex flex-wrap mb-3">
            <div class="w-full md:w-1/2 px-3 mb-3 md:mb-0" :class="{ 'form-group--error': $v.firstName.$error }">
              <label class=" tracking-wide text-gray-700 text-sm font-semibold mb-2">
                First Name
              </label>
              <input type="text" v-model.trim="$v.firstName.$model" class="appearance-none border rounded w-full py-3 px-3 text-gray-700 mt-1 leading-tight focus:outline-none focus:shadow-outline"  required>         
              <span class="text-red-500 text-xs" v-if="$v.firstName.$error && !$v.firstName.required">First name is required</span>
            </div>

            <div class="w-full md:w-1/2 px-3" :class="{ 'form-group--error': $v.lastName.$error }">
              <label class=" tracking-wide text-gray-700 text-sm font-semibold mb-2">
                Last Name
              </label>
              <input type="text" v-model.trim="$v.lastName.$model" class="appearance-none border rounded w-full py-3 px-3 text-gray-700 mt-1 leading-tight focus:outline-none focus:shadow-outline"  required>         
              
              <span class="text-red-500 text-xs" v-if="$v.lastName.$error && !$v.lastName.required">Last name is required</span>
            </div>

          </div>
          <div class="w-full md:w-full px-3 mb-3">
            <label class="tracking-wide text-gray-700 text-sm font-semibold mb-2">Email Address</label>
            <input type="email" v-model.trim="$v.email.$model" :class="{ 'is-invalid': $v.email.$error , 'is-valid': !$v.email. $invalid}" class="appearance-none border rounded w-full py-3 px-3 text-gray-700 mt-1 leading-tight focus:outline-none focus:shadow-outline"  required>         
            <span class="text-red-500 text-xs invalid-feedback" v-if="$v.email.$error && !$v.email.required">Email is required</span>
            <span class="text-red-500 text-xs invalid-feedback" v-if="$v.email.$error && !$v.email.isUnique">Email already exists</span>
          </div>

          <div class="w-full md:w-full px-3 mb-6" :class="{ 'error': $v.password.$error }">
            <label class="tracking-wide text-gray-700 text-sm font-semibold mb-2">Password</label>
            <input type="password" v-model.trim="$v.password.$model" class="appearance-none border rounded w-full py-3 px-3 text-gray-700 mt-1 leading-tight focus:outline-none focus:shadow-outline"  required>         
            
            <span class="text-red-500 text-xs" v-if="$v.password.$error && !$v.password.required">Password is required</span>
            <span class="text-red-500 text-xs" v-if="$v.password.$error && !$v.password.between">Password must be {{$v.password.$params.between.max}} characters long.</span>
          </div>

          <div class="w-full md:w-full px-3">
            <button @click="login" type="submit"  class="appearance-none block w-full bg-purple-700 text-gray-100 font-bold border border-gray-200 rounded-lg py-3 px-3 leading-tight hover:bg-purple-900 focus:outline-none focus:bg-purple-500 focus:border-gray-500">Sign in {{submitstatus}}</button>
         </div>
      </div>
    </div>
  </div>
</div>

</template>

<script>
import axios from 'axios';
import { required, between, email } from 'vuelidate/lib/validators';

export default {
  name: 'Login',
  data(){
    return{
        firstName:'',
        lastName: '',
        email:'',
        password:'',
    }
  },
  validations:{
    firstName:{
      required,
    },
    lastName:{
      required,
    },
    password:{
      required,
      between: between(3,8)
    },
    email:{
      required,
      email,
      isUnique(value){
        if(value === '') return true
        let email_regex = /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$/
        return new Promise((resolve)=>{
          setTimeout(()=>{
            resolve(email_regex.test(value))
          }, 400 + Math.random()*300)
        })
      }
    }
  },
  computed: {
    hasFilledForm: function(){
      return !this.validations.firstName;
    }
  },
  methods: {
    login () { 
      this.$v.$touch()
      axios.post('https://api.raisely.com/v3/signup',{ 
        "campaignUuid": "46aa3270-d2ee-11ea-a9f0-e9a68ccff42a",
        "data":{
          firstName: this.firstName, 
          lastName: this.lastName, 
          email: this.email, 
          password: this.password 
        }
      })
      .then(response => console.log(response.data))
      .catch(error => console.log(error));
        if(this.$v.$invalid){
          this.submitstatus = 'Failed'
        } else{
          this.submitstatus = 'Successful'
        } 
    },
  }
}
</script>
