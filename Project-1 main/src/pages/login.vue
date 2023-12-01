<template>
  <div>
    <h1>Login Page</h1>
    <div class="flex flex-col gap-2">
      <input
        type="text"
        required
        v-model="username"
        class="border"
        placeholder="Username"
      />
      <input
        type="password"
        required
        v-model="password"
        class="border"
        placeholder="Password"
      />
      <button
        @click="onLogin()"
        :disabled="!isFormValid"
        class="bg-blue-500 text-white py-1 px-3"
      >
        Login
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useRouter } from "vue-router";
import { useAuthStore } from "@/stores/auth";
import axios from "axios";
const auth = useAuthStore();
const username = ref("");
const password = ref("");
const router = useRouter();

// Check if both username and password are not empty
const isFormValid = () => {
  return username.value.trim() !== "" && password.value.trim() !== "";
};

const onLogin = async () => {
  if(username.value === "" || password.value === "") {
    alert("Username or Password can't be empty")
  } else {
  try {
    const response = await axios.post('http://localhost:3000/login', {
      username: username.value,
      password: password.value,
    })
    
    auth.login(username.value);
    console.log(response);
  } catch (error) {
    console.error('Login error:', error);
  }
}
};
</script>
