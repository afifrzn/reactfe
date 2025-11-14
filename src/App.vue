<template>
  <div class="container">
    <h2>Login Rust Demo (Vue)</h2>

    <form @submit.prevent="handleLogin">
      <input v-model="username" type="text" placeholder="Username" required />
      <input v-model="password" type="password" placeholder="Password" required />
      <button type="submit">Login</button>
    </form>

    <p :style="{ color: messageColor }">{{ message }}</p>
  </div>
</template>

<script setup>
import { ref } from "vue";

const username = ref("");
const password = ref("");
const message = ref("");
const messageColor = ref("");

/**
 * HANDLE POST /LOGIN
 * - Mengirim username + password ke server Rust
 * - Menerima respons JSON apakah login berhasil / gagal
 */
const handleLogin = async () => {
  try {
    const res = await fetch("http://rustapp:8080/login", {
      method: "POST", // <-- POST ke endpoint login
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        username: username.value,
        password: password.value,
      }),
    });

    const data = await res.json();

    message.value = data.message || "Login berhasil!";
    messageColor.value = data.success ? "green" : "red";
  } catch (err) {
    console.error(err);
    message.value = "Gagal terhubung ke server!";
    messageColor.value = "gray";
  }
};
</script>

<style>
.container {
  max-width: 400px;
  margin: 50px auto;
  text-align: center;
}
input {
  display: block;
  width: 100%;
  padding: 10px;
  margin: 8px 0;
}
button {
  padding: 10px 20px;
  cursor: pointer;
}
</style>