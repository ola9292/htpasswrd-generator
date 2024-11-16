<script setup>
import { ref, computed } from 'vue';
import bcrypt from 'bcryptjs';

const username = ref('')
const password = ref('')
const hashedPassword = ref('')
const randomPassword = ref('')

//hash password
const hashPassword = async () => {
   if(!password.value && !username.value){
    alert('enter a username and password');
   }else if(!password.value){
    alert('enter a password');
   }else if(!username.value){
    alert('enter a username');
   }else{
    const saltRounds = 10;
    hashedPassword.value = await bcrypt.hash(password.value, saltRounds);
   }
   
};

//generate random password
const getRandomPass = () => {
    const possibleOptions = [
  ...'abcdefghijklmnopqrstuvwxyz', // Small letters
  ...'ABCDEFGHIJKLMNOPQRSTUVWXYZ', // Capital letters
  ...'0123456789',                 // Numbers
  ...'!@#$%^&*()_+[]{}|;:,.<>?/`~' // Symbols
];
const pword = [];
let newPword = '';
    for(let i = 0; i < 10; i++){
        const rand = Math.floor(Math.random() * possibleOptions.length)
        pword.push(possibleOptions[rand]);
    }
    newPword = pword.join('')
    password.value = newPword
}

//clear input
const clearInput = () => {
  username.value = '';
  password.value = '';
}
//final copy
const finalCopy = computed(() => {
  if(username.value && hashedPassword.value){
    return `${username.value}:${hashedPassword.value}`
  }else{
    return "expected output here"
  }
  
})
</script>

<template>

  <h2>HTPasswd Generator</h2>
  
  <div class="container">
    <div class="heading-1">
      <h4><i class="fa-solid fa-right-from-bracket"></i> Input <span>enter a name and password</span></h4>
      <button class="clear-btn" @click="clearInput"><i class="fa-solid fa-xmark"></i> Clear</button>
    </div>
    <div class="username">
      <label for="">Username</label>
      <input type="text" placeholder="Name" v-model="username">
    </div>

    <div class="password">
      <label for="">Password</label>
      <div class="password-input">
        <input type="text" placeholder="password to be encrypted" v-model="password">
        <button @click="getRandomPass" class="random"><i class="fa-solid fa-arrow-left"></i> Random</button>
      </div>
    </div>

    <div class="generate-password">
        <button @click="hashPassword" class="generate-hash-btn">Generate .htpasswd file</button>
      </div>
  </div>
  <div class="container">
    <div class="heading-2">
      <h4><i class="fa-solid fa-right-from-bracket"></i> Output <span>copy the entry bellow into your .htpasswd file</span></h4>
    </div>
    <div>
      <p>{{ finalCopy }}</p>
    </div>
  </div>
</template>
 
<style scoped>

  .container{
    --primary-color: #d9d9d9;  /* Blue */
  --secondary-color: #009999; /* Green */
  --color-white: #ffffff;
  --color-black: #000000;
  --color-red:#990000;
  --text-color: #333;       /* Dark gray */
  --background-color: #f4f4f4; /* Light gray */
  font-family: sans-serif;

    width: 800px;
    max-width: 90%;
    border: 1px solid var(--primary-color);
    margin: auto;
    padding: 10px;
    display: grid;
    gap: 20px;
    border-radius: 5px;
    margin-bottom: 10px;
    overflow: auto;
    
  }
  h2{
    text-align: center;
   
  }
  .container > h2{
    text-align: center;
  }
  input, .random, .generate-hash-btn{
    padding: 8px 4px;
    border: 1px solid #d9d9d9;
    
  }
  .username,.password,.generate-password{
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  .generate-hash-btn{
    background-color: #4775d1;
    color:var(--color-white)
  }
  .password-input{
    display: flex;

  }
  .password-input > input{
    flex: 1;
  }
  .random{
    background-color: #ffb31a;
    border: 1px solid #ffb31a;
    color: var(--color-white);
  }
  .heading-1{
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: var(--secondary-color);
  }
  .heading-2{
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: var(--color-red);
  }
  .clear-btn{
    padding: 4px 8px;
    border-radius: 15px;
    border: 1px solid var(--primary-color);
    background-color: transparent;
  }
</style>
