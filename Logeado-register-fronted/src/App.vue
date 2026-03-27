<script setup>
import { ref } from 'vue';

const view = ref('login'); // inicia en login

const name = ref('');
const email = ref('');
const password = ref('');
const message = ref('');

const API = 'http://localhost:3000/api/users';

// REGISTER
const register = async () => {
  try {
    const res = await fetch(API, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        name: name.value,
        email: email.value,
        password: password.value
      })
    });

    const data = await res.json();
    message.value = data.message || 'Usuario registrado';
  } catch (err) {
    message.value = 'Error en registro';
  }
};

// LOGIN
const login = async () => {
  try {
    const res = await fetch(API + '/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        email: email.value,
        password: password.value
      })
    });

    const data = await res.json();
    message.value = data.message;
  } catch (err) {
    message.value = 'Error en login';
  }
};
</script>

<template>
  <div style="text-align:center; margin-top:50px;">

    <!-- BOTONES ARRIBA -->
    <div>
      <button @click="view = 'login'">Iniciar Sesión</button>
      <button @click="view = 'register'">Registrarse</button>
    </div>

    <hr><br>

    <!-- LOGIN -->
    <div v-if="view === 'login'">
      <h2>Login</h2>

      <input v-model="email" placeholder="Email" /><br><br>
      <input v-model="password" type="password" placeholder="Contraseña" /><br><br>

      <button @click="login">Entrar</button>
    </div>

    <!-- REGISTER -->
    <div v-else>
      <h2>Registro</h2>

      <input v-model="name" placeholder="Nombre" /><br><br>
      <input v-model="email" placeholder="Email" /><br><br>
      <input v-model="password" type="password" placeholder="Contraseña" /><br><br>

      <button @click="register">Crear cuenta</button>
    </div>

    <br>
    <p>{{ message }}</p>

  </div>
</template>