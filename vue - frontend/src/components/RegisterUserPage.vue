<template>
  <div class="splitter-container">
    <div class="background-image"></div>
    <div class="wrapper">
      <div class="logo">
        <!-- Background image is set in CSS -->
      </div>
      <div class="text-center mt-4 name">
        User Registration
      </div>
      <form @submit.prevent="registerUser" class="p-3 mt-3">
        <div class="form-field d-flex align-items-center">
          <span class="far fa-user"></span>
          <input type="text" name="name" v-model="name" class="form-control" placeholder="Name">
        </div>
        <div class="form-field d-flex align-items-center">
          <span class="far fa-envelope"></span>
          <input type="email" name="email" v-model="email" class="form-control" placeholder="Email">
        </div>
        <div class="form-field d-flex align-items-center">
          <span class="fas fa-key"></span>
          <input type="password" name="password" v-model="password" class="form-control" placeholder="Password">
        </div>
        <div class="form-field d-flex align-items-center">
          <span class="fas fa-key"></span>
          <input type="password" name="password_confirmation" v-model="password_confirmation" class="form-control" placeholder="Confirm Password">
        </div>
        <button type="submit" class="btn mt-3">Register</button>
      </form>
      <div class="text-center fs-6">
        <p class="mt-3">Already have an account? <router-link to="/" class="link-sign">Login here!</router-link></p>
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
          // Reset fields
          this.name = '';
          this.email = '';
          this.password = '';
          this.password_confirmation = '';
          alert('New account created.');
          this.$router.go(-1);
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
.splitter-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f0f0; /* Ensure a background color in case the image doesn't load */
  position: relative;
}

.background-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('https://antdisplay.com/pub/media/wysiwyg/63.jpg');
  background-size: cover;
  background-position: center;
  z-index: -1; /* Place it behind other content */
}

.wrapper {
  width: 100%;
  max-width: 400px;
  padding: 40px 30px 30px 30px;
  background-color: rgba(236, 240, 243, 0.9);
  border-radius: 15px;
  box-shadow: 13px 13px 20px #cbced1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative; /* Ensure the wrapper is above the background image */
  z-index: 1;
}

.logo {
  width: 120px;
  height: 120px;
  background-image: url('@/assets/logo-ecommerce.png');
  background-size: cover;
  background-position: center;
  border-radius: 50%;
  margin: 0 auto;
  box-shadow: 0px 0px 3px #5f5f5f,
              0px 0px 0px 5px #ecf0f3,
              8px 8px 15px #a7aaa7,
              -8px -8px 15px #fff;
}

.name {
  font-weight: 600;
  font-size: 1.4rem;
  letter-spacing: 1.3px;
  color: #555;
}

.form-field input {
  width: 100%;
  display: block;
  border: none;
  outline: none;
  background: none;
  font-size: 1.2rem;
  color: #666;
  padding: 10px 15px 10px 10px;
}

.form-field {
  width: 100%;
  padding-left: 10px;
  margin-bottom: 20px;
  border-radius: 20px;
  box-shadow: inset 8px 8px 8px #cbced1, inset -8px -8px 8px #fff;
}

.form-field .fas,
.form-field .far {
  color: #555;
}

.form-field input:focus {
  box-shadow: none;
}

.btn {
  width: 100%;
  height: 40px;
  letter-spacing: 1.3px;
  border-radius: 25px;
  font-size: 18px;
  box-shadow: rgba(45, 35, 66, 0.5) 0 2px 4px, rgba(45, 35, 66, 0.5) 0 7px 13px -3px, #D6D6E7 0 -3px 0 inset;
  background-color: #808080; /* Changed to gray */
  border: none;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn:hover {
  background: linear-gradient(to bottom, #a9a9a9, white); /* Adjusted for a gray gradient */
  color: black;
  border: 1px solid #696969; /* Adjusted border color */
  transform: scale(1.02);
  border-radius: 5rem;
}

a {
  text-decoration: none;
  font-size: 0.8rem;
  font-size: 1rem;
  color: rgb(5, 99, 193); 
}

a:hover {
  text-decoration: underline;
  transform: scale(1.1); 
  color: #039BE5;
}

@media(max-width: 768px) {
  .background-image {
    display: none; 
  }

  .wrapper {
    padding: 20px; 
  }
}

@media(max-width: 480px) {
  .logo {
    width: 120px;
    height: 120px;
  }

  .name {
    font-size: 1.2rem; 
  }

  .btn {
    font-size: 16px; 
  }
}
</style>