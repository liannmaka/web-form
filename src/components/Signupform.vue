<template>
  <form @submit.prevent="handleForm">
        <label for="email">Email:</label>
        <div :class="{ error: v$.email.$error}">
          <input v-model="email" type="email" id="email">
          <div v-for="error in v$.email.$errors" :key="error.$uid">
          <p>{{ error.$message }}</p>
          </div>
        </div>
        
       <label for="password">Password:</label>
       <div :class="{ error: v$.password.$error}">
          <input type="password" v-model="password" id="password">
          <div v-for="error in v$.password.$errors" :key="error.$uid">
              <p>{{ error.$message }}</p>
          </div>
       </div>

      <label for="role">Role:</label>
      <div :class="{error: v$.role.$error}">
         <select v-model="role" id="role">
          <option value="developer">Web developer</option>
          <option value="designer">Web designer</option>
         </select>
          <p v-for="error in v$.role.$errors" :key="error.$uid">{{ error.$message }}</p>
      </div>
      
      <label>Skills:</label>
      <input type="text" v-model="tempskill" @keyup.alt="addSkill">
      <div v-for="skill in skills" :key="skill" class="pill">
          <span @click="deleteSkill(skill)">{{ skill }}</span>
      </div>

      <div class="terms">
          <input type="checkbox" v-model="terms">
          <label>Accept Terms and Conditions</label>
      </div>
      <div class="submit">
          <button type="submit">Create an account</button>
      </div>
  </form>
</template>

<script>
import useVuelidate from '@vuelidate/core'
import { required, email, minLength, helpers } from '@vuelidate/validators'

export default {
setup (){
    return{
        v$: useVuelidate() 
    }
},
data (){
    return{
        email: '',
        password: '',
        role: '',
        terms: true,
        tempskill: '',
        skills:[],
        currentPage: 5,
        rows: 200,
        perPage: 15
    }
},
validations (){
   return {
        email: { required: helpers.withMessage('Email is required', required), email: helpers.withMessage('Email is not valid', email), $autoDirty: true },

        password: { required: helpers.withMessage('Password is required', required), minLength: helpers.withMessage('Password should be at least 6 characters long', minLength(6)), $autoDirty: true },

        role: { required: helpers.withMessage('Role is required', required) },

        terms: { required: helpers.withMessage('Accept terms and Conditions', required) }
   }
}, 
methods:{
    addSkill(e){
        if(e.key === ',' && this.tempskill){
            if(!this.skills.includes(this.tempskill)){
            this.skills.push(this.tempskill)
            }  
            this.tempskill = ''
        }
    },
    deleteSkill(skill){
        this.skills = this.skills.filter((item) => {
            return skill !== item
        })
    },
    handleForm(){
        this.v$.$validate();
        }
    },
}

</script>

<style>
form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}
label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}
input, select {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;

}
input[type="checkbox"] {
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px;
}
input:focus{
    border: red;

}
.pill {
    display: inline-block;
    margin: 20px 10px 0 0;
    padding: 6px 12px;
    background: #eee;
    border-radius: 20px;
    font-size: 12px;
    letter-spacing: 1px;
    font-weight: bold;
    color: #777;
    cursor: pointer;
}
button {
    background: #0b6dff;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
}
.submit {
    text-align: center;
}
.error {
    color: #ff0062;
    margin-top: 10px;
    font-size: 0.8em;
    font-weight: bold;
}
</style>