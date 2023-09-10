<template>
    <div class="login-page">
      <h2>Iniciar sesión</h2>
      <form @submit.prevent="login">
        <div class="form-group">
          <label for="username">Nombre de usuario</label>
          <input type="text" id="username" v-model="username" required>
        </div>
        <div class="form-group">
          <label for="password">Contraseña</label>
          <input type="password" id="password" v-model="password" required>
        </div>
        <button type="submit">Iniciar sesión</button>
      </form>
      <p v-if="error" class="error-message">{{ error }}</p>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        username: '',
        password: '',
        error: '',
      };
    },
    methods: {
      login() {
        if (this.username === 'usuario' && this.password === 'contraseña') {
          console.log('Inicio de sesión exitoso');
        } else {
          this.error = 'Credenciales incorrectas';
        }
      },
    },
  };
  </script>
  
  <style>
  .login-page {
    max-width: 300px;
    margin: auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  .form-group {
    margin-bottom: 10px;
  }
  .error-message {
    color: red;
  }
  </style>
  