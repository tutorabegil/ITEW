<template>
  <div>
    <NavBar></NavBar>
    <div class="splitter-container">
      <!-- Wrapper -->
      <div class="wrapper">
        <div class="text-center mt-4 name">
          Add Product
        </div>
        <form @submit.prevent="addProduct" class="p-3 mt-3">
          <div class="form-field d-flex align-items-center mb-3">
            <span class="fas fa-tag"></span>
            <input type="text" class="form-control" id="name" v-model="name" placeholder="Enter name" required>
          </div>
          <div class="form-field d-flex align-items-center mb-3">
            <span class="fas fa-info-circle"></span>
            <textarea class="form-control" v-model="description" id="description" rows="3" placeholder="Description" required></textarea>
          </div>

          <div class="form-field d-flex align-items-center mb-3">
            <span class="fas fa-dollar-sign"></span>
            <input type="number" class="form-control" step="0.01" v-model="price" placeholder="Price" required />
          </div>

          <div class="form-field d-flex align-items-center mb-3">
            <span class="fas fa-boxes"></span>
            <input type="number" class="form-control" v-model="stocks" placeholder="Stocks" required>
          </div>

          <div class="d-flex align-items-center justify-content-center">
            <button type="submit" class="btn mt-3 btn-success">Save</button>
            <router-link to="/home" class="btn mt-3 btn-secondary ml-2">Cancel</router-link>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>


<script>
import axios from '@/lib/axios';
import NavBar from '@/components/NavBar.vue';

export default {
  components: {
    NavBar
  },
  methods: {
    async addProduct() {
      try {
        // check if product already exists
        let products = this.$store.getters.getProducts;
        for (let i = 0; i < products.length; i++) {
          if (this.name.toLowerCase() === products[i].name.toLowerCase()) {
            throw "Product already exists";
          }
        }
        const response = await axios.post('/api/products/store', {
          name: this.name,
          description: this.description,
          stocks: this.stocks,
          user_id: this.$store.getters.getAccountDetails.id,
          price: this.price
        });

        if (response.status === 201) {
          this.$store.dispatch('asyncAddProduct', response.data.product);
          alert('Product created');

          this.$router.push('/home');
        }
      } catch (error) {
        alert(error);
      }
    }
  },
  data() {
    return {
      name: '',
      description: '',
      price: 0.00,
      stocks: 0
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
  background-color: #f0f0f0; /* Optional background color */
}

.wrapper {
  width: 100%;
  max-width: 400px;
  padding: 30px;
  background-color: #ecf0f3;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.name {
  font-weight: 600;
  font-size: 1.4rem;
  color: #555;
}

.form-field input,
.form-field textarea {
  width: 100%;
  display: block;
  border: none;
  outline: none;
  background: none;
  font-size: 1.2rem;
  color: #666;
  padding: 10px;
}

.form-field {
  margin-bottom: 20px;
  border-radius: 8px;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.form-field .fas {
  color: #555;
}

.btn {
  width: 100%;
  height: 40px;
  border-radius: 20px;
  font-size: 18px;
  background-color: #6c757d;
  border: none;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-success:hover,
.btn-secondary:hover {
  background: #5a6268;
  color: white;
  transform: scale(1.02);
}

.btn-secondary {
  background-color: #6c757d;
}

.btn-secondary:hover {
  background-color: #5a6268;
}

@media(max-width: 480px) {
  .name {
    font-size: 1.2rem;
  }

  .btn {
    font-size: 16px;
  }
}
</style>
