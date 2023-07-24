<template>
  <div class="register-container">
    <h1 class="register-title">Register Page</h1>
    <form @submit.prevent="register" class="register-form">
      <label class="register-label">
        Username:
        <input type="text" v-model="username" required class="register-input">
      </label>
      <br>
      <label class="register-label">
        Password:
        <input type="password" v-model="password" required class="register-input">
      </label>
      <br>
      <button type="submit" class="register-button" @click="redirectToLogin">Register</button>
    </form>
    <p>Already have an account? <a @click="redirectToLogin">Login</a></p>
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
    async register() {
      try {
        const response = await axios.post('https://anna-be.vercel.app/register', {
          username: this.username,
          password: this.password
        });

        if (response.status === 200) {
          console.log(response.data.message);
          // Display a success message to the user

          // Redirect the user to the login page
          this.$router.push('/login');
        } else {
          console.error(response.data.error);
          // Display an error message to the user
        }
      } catch (error) {
        console.error(error);
        // Display an error message to the user
      }
    },
    redirectToLogin() {
      this.$router.push('/login');
    }
  }
};
</script>

<style scoped>
/* Styling for Register Page (Halaman Pendaftaran) */

.register-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 50px;
}

.register-title {
  font-size: 24px;
  margin-bottom: 20px;
}

.register-form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.register-label {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  font-size: 18px;
  margin-bottom: 10px;
}

.register-input {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
}

.register-button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 18px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.register-button:hover {
  background-color: #45a049;
}

.register-container p {
  margin-top: 10px;
}

.register-container p a {
  cursor: pointer;
  color: #007bff;
  text-decoration: underline;
}

.register-container p a:hover {
  color: #0056b3;
  text-decoration: underline;
}
</style>
