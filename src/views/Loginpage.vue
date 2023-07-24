<template>
  <div class="login-container">
    <h1 class="login-title">Login Page</h1>
    <form @submit.prevent="login" class="login-form">
      <label class="login-label">
        Username:
        <input type="text" v-model="username" required class="login-input">
      </label>
      <br>
      <label class="login-label">
        Password:
        <input type="password" v-model="password" required class="login-input">
      </label>
      <br>
      <button type="submit" class="login-button" @click="redirectTodata">Login</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      username: '',
      password: ''
    };
  },
  methods: {
    async login() {
      try {
        const response = await axios.post('https://anna-be.vercel.app/login', {
          username: this.username,
          password: this.password
        });

        if (response.status === 200) {
          const { token } = response.data;
          // Save the token to local storage as a cookie
          localStorage.setItem('token', token);

          // Redirect the user to the Data Rencana page
          this.$router.push('/rencana');
        } else {
          console.error(response.data.error);
          // Display an error message to the user
        }
      } catch (error) {
        console.error(error);
        // Display an error message to the user
      }
    }
  }
};
</script>

<style scoped>
/* Styling for Login Page (Halaman Masuk) */
.login-container {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
  text-align: center;
}

.login-title {
  font-size: 24px;
  margin-bottom: 20px;
  color: #007bff;
}

.login-form {
  display: flex;
  flex-direction: column;
}

.login-label {
  margin-bottom: 10px;
  font-weight: bold;
  color: #333;
}

.login-input {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.login-button {
  padding: 10px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.login-button:hover {
  background-color: #0056b3;
}
</style>
