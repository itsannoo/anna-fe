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
      <button type="submit" class="login-button">Login</button>
    </form>
    <p v-if="error" class="error-message">{{ error }}</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      username: '',
      password: '',
      error: null
    };
  },
  methods: {
    async login() {
  try {
    const response = await axios.post('https://anna-be.vercel.app/login', {
      username: this.username,
      password: this.password
    });

    const { token } = response.data;
    // Save the token to local storage as a cookie
    localStorage.setItem('token', token);

    // Redirect the user to the Data Rencana page
    this.$router.push('/rencana');
  } catch (error) {
    console.error(error);
    if (error.response && error.response.status === 401) {
      this.error = 'Invalid username or password. Please try again.';
    } else {
      this.error = 'Failed to login. Please try again later.';
    }
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
