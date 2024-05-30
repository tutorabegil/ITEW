<template>
  <div class="container">
    <div class="form-wrapper">
      <div class="logo">
        <img :src="require('@/assets/logo-ecommerce.png')" alt="E-commerce Logo">
      </div>
      <h2 class="title">User Registration</h2>
      <form @submit.prevent="registerUser" class="form">
        <div class="form-group">
          <input type="text" name="name" v-model="name" class="form-control" placeholder="Name" required>
        </div>
        <div class="form-group">
          <input type="email" name="email" v-model="email" class="form-control" placeholder="Email" required>
        </div>
        <div class="form-group">
          <input type="password" name="password" v-model="password" class="form-control" placeholder="Password" required>
        </div>
        <div class="form-group">
          <input type="password" name="password_confirmation" v-model="password_confirmation" class="form-control" placeholder="Confirm Password" required>
        </div>
        <button type="submit" class="btn">Register</button>
      </form>
      <div class="login-link">
        <p>Already have an account? <router-link to="/" class="link">Login here!</router-link></p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from '@/lib/axios';

export default {
  methods: {
    async registerUser() {
      try {
        const response = await axios.post('/api/create', {
          name: this.name,
          email: this.email,
          password: this.password,
          password_confirmation: this.password_confirmation,
        });

        if (response.status === 201) {
          this.name = '';
          this.email = '';
          this.password = '';
          this.password_confirmation = '';
          alert('New account created.');
          this.$router.push('/');
        }
      } catch (error) {
        console.log(error);
        alert('Registration failed. Please try again.');
      }
    }
  },
  data() {
    return {
      name: '',
      email: '',
      password: '',
      password_confirmation: '',
    };
  }
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f2f5;
}

.form-wrapper {
  width: 100%;
  max-width: 400px;
  padding: 20px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.logo img {
  width: 80px;
  height: 80px;
  object-fit: cover;
  margin-bottom: 20px;
}

.title {
  font-size: 1.5rem;
  margin-bottom: 20px;
  color: #333;
}

.form {
  display: flex;
  flex-direction: column;
}

.form-group {
  margin-bottom: 15px;
}

.form-control {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
}

.btn {
  width: 100%;
  padding: 10px;
  background-color: #007bff;
  border: none;
  border-radius: 5px;
  color: white;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #0056b3;
}

.login-link {
  margin-top: 20px;
  font-size: 0.9rem;
  color: #333;
}

.login-link a {
  color: #007bff;
  text-decoration: none;
}

.login-link a:hover {
  text-decoration: underline;
}
</style>
