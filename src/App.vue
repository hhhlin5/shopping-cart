<template>
  <div id="app">
    <div class="cart-container">
      <div class="cart-header">Cart</div>
      
      <div class="cart-body">
        <table class="cart-table">
          <thead>
            <tr>
              <th>Name</th>
              <th>Quantity</th>
              <th>(Standard) Price (USD)</th>
              <th>Sub-total (USD)</th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="item in cartItems" :key="item.id">
              <td>{{ item.name }}</td>
              <td>
                <input 
                  type="number" 
                  v-model.number="item.qty" 
                  min="1" 
                  class="form-control quantity-input"
                  @change="updateQuantity(item)"
                >
              </td>

              <td>{{ item.price }}</td>
              <td>{{ (item.qty * item.price).toFixed(2) }}</td>
            </tr>
            <tr class="total-row">
              <td colspan="3" class="text-end"><strong>Total</strong></td>
              <td><strong>USD {{ totalAmount.toFixed(2) }}</strong></td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="cart-footer">
        <div class="d-flex gap-2 justify-content-end">
          <button @click="resetCart" class="btn btn-reset">Reset</button>
          <button @click="checkout" class="btn btn-checkout">Checkout</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      cartItems: [
        { id: 1, name: "Chicken Wing", category: "Food", qty: 3, price: 10 },
        { id: 2, name: "Pizza", category: "Food", qty: 1, price: 50 },
        { id: 3, name: "Hamburger", category: "Food", qty: 1, price: 12 },
        { id: 4, name: "Coca Cola", category: "Drink", qty: 2, price: 5 },
        { id: 5, name: "Orange Juice", category: "Drink", qty: 1, price: 15 },
        { id: 6, name: "Potato Chips", category: "Snack", qty: 1, price: 8 }
      ],
      originalItems: [
        { id: 1, name: "Chicken Wing", category: "Food", qty: 0, price: 10 },
        { id: 2, name: "Pizza", category: "Food", qty: 0, price: 50 },
        { id: 3, name: "Hamburger", category: "Food", qty: 0, price: 12 },
        { id: 4, name: "Coca Cola", category: "Drink", qty: 0, price: 5 },
        { id: 5, name: "Orange Juice", category: "Drink", qty: 0, price: 15 },
        { id: 6, name: "Potato Chips", category: "Snack", qty: 0, price: 8 }
      ]
    }
  },
  computed: {
    totalAmount() {
      return this.cartItems.reduce((total, item) => {
        return total + (item.qty * item.price);
      }, 0);
    }
  },
  methods: {
    updateQuantity(item) {
      if (item.qty < 1) {
        item.qty = 1;
      }
    },
    resetCart() {
      this.cartItems = JSON.parse(JSON.stringify(this.originalItems));
    },
    checkout() {
      alert(`Checkout successful! Total amount: USD ${this.totalAmount.toFixed(2)}`);
      this.resetCart();
    }
  }
}
</script>

<style>
  .cart-container {
  max-width: 800px;
  margin: 40px auto;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  padding: 0;
}
.cart-header {
  background: #f8f9fa;
  padding: 20px;
  border-bottom: 1px solid #dee2e6;
  font-weight: 600;
  font-size: 18px;
}
.cart-table {
  margin: 0;
  width: 100%;
}
.cart-table th {
  background: #f8f9fa;
  padding: 15px;
  font-weight: 600;
  border-bottom: 2px solid #dee2e6;
}
.cart-table td {
  padding: 15px;
  vertical-align: middle;
  border-bottom: 1px solid #dee2e6;
}
.quantity-input {
  width: 60px;
  text-align: center;
}
.cart-footer {
  background: #f8f9fa;
  padding: 20px;
  border-top: 1px solid #dee2e6;
}
.total-row {
  font-weight: 600;
  background: #e9ecef;
}
.btn-reset {
  background: #6c757d;
  border-color: #6c757d;
  color: white;
}
.btn-reset:hover {
  background: #5a6268;
  border-color: #545b62;
  color: white;
}
.btn-checkout {
  background: #007bff;
  border-color: #007bff;
  color: white;
}
.btn-checkout:hover {
  background: #0069d9;
  border-color: #0062cc;
  color: white;
}
</style>