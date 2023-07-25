<template>
  <div>
    <header class="header">
      <img
        alt="Custom Logo"
        class="logo"
        src="https://pngimg.com/uploads/minions/minions_PNG42.png"
        width="150"
        height="150"
      />

      <nav class="nav-links">
        <RouterLink to="/register" class="nav-link">Register</RouterLink>
        <RouterLink to="/login" class="nav-link">Login</RouterLink>
      </nav>
    </header>

    <div class="form-container">
      <RouterView />
    </div>
  </div>
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router'
import axios from 'axios';

const isLoggedIn = !!localStorage.getItem('token');

const registerUsername = '';
const registerPassword = '';
const loginUsername = '';
const loginPassword = '';

let showRegisterForm = true;

const register = async () => {
  try {
    const response = await axios.post('https://anna-be.vercel.app/register', {
      username: registerUsername,
      password: registerPassword
    });

    if (response.status === 200) {
      console.log(response.data.message);
      
      showRegisterForm = false; // Hide the register form after successful registration
    } else {
      console.error(response.data.error); 
    }

    
  } catch (error) {
    console.error(error);
    // Display an error message to the user
  }
};

const login = async () => {
  try {
    const response = await axios.post('https://anna-be.vercel.app/login', {
      username: loginUsername,
      password: loginPassword
    });

    if (response.status === 200) {
      const { token } = response.data;
      // Save the token to local storage as a cookie
      localStorage.setItem('token', token);

      // Redirect the user to the Data Rencana page
      $router.push('/rencana');
    } else {
      console.error(response.data.error);
      // Display an error message to the user
    }
  } catch (error) {
    console.error(error);
    // Display an error message to the user
  }
};
</script>

<style scoped>
/* Global CSS, styling, dan lainnya */

body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

.header {
  background-color: #020760;
  padding: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
}

.logo {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.1);
}

.nav-links {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 2rem;
}

.nav-link {
  display: inline-block;
  padding: 1rem 1.5rem;
  font-size: 1rem;
  color: #333;
  text-decoration: none;
  border-radius: 4px;
  margin: 0 0.5rem;
  background-color: #f2f2f2;
  transition: background-color 0.2s;
}

.nav-link.router-link-exact-active {
  background-color: #007bff;
  color: #fff;
}

.form-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin-top: 2rem;
}

.form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.form-title {
  font-size: 24px;
  margin-bottom: 20px;
}

.form-label {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  font-size: 18px;
  margin-bottom: 10px;
}

.form-input {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
  width: 300px;
}

.form-button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 18px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.form-button:hover {
  background-color: #45a049;
}

.login-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.login-container p {
  margin-top: 10px;
}

.login-container p a {
  cursor: pointer;
  color: #007bff;
  text-decoration: underline;
}

.login-container p a:hover {
  color: #0056b3;
  text-decoration: underline;
}

.register-container {
  display: flex;
  flex-direction: column;
  align-items: center;
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
