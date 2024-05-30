<template>
  <div class="container">
    <div class="login-wrapper">
      <div class="logo">
        <img :src="require('@/assets/logo-ecommerce.png')" alt="E-commerce Logo" class="logo-img">
      </div>
      <h2 class="title">Login</h2>
      <form @submit.prevent="login" class="login-form">
        <div class="form-group">
          <input type="text" name="email" v-model="email" class="form-control" placeholder="Email" required>
        </div>
        <div class="form-group">
          <input type="password" name="password" v-model="password" class="form-control" placeholder="Password" required>
        </div>
        <button type="submit" class="btn">Login</button>
      </form>
      <div class="register-link">
        <p>Don't have an account? <router-link to="/register">Sign up here!</router-link></p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "@/lib/axios";

export default {
  methods: {
    async login() {
      try {
        const response = await axios.post('/api/login', {
          email: this.email,
          password: this.password
        });

        if (response.status === 200) {
          localStorage.setItem('token', response.data.token);
          this.email = '';
          this.password = '';

          this.$router.push('/home');
        }
      } catch (error) {
        console.log(error);
      }
    }
  },
  data() {
    return {
      email: '',
      password: ''
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
  background: #f0f2f5;
}

.login-wrapper {
  width: 100%;
  max-width: 350px;
  padding: 20px;
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.logo {
  margin-bottom: 20px;
}

.logo-img {
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 50%;
}

.title {
  font-size: 1.5rem;
  margin-bottom: 20px;
  color: #333;
}

.login-form {
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

.register-link {
  margin-top: 20px;
  font-size: 0.9rem;
  color: #333;
}

.register-link a {
  color: #007bff;
  text-decoration: none;
}

.register-link a:hover {
  text-decoration: underline;
}
</style>
