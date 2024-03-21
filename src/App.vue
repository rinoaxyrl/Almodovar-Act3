<template>
  <div id="app">
    <h1>Shopping Cart</h1>
    <hr>

    <div>
      <h2>Products</h2>
      <ul>
        <li v-for="product in products" :key="product.id">
          {{ product.name }} - ₱{{ product.price }}
          <button class="add-to-cart" @click="addToCart(product)">Add to Cart</button>
        </li>
      </ul>
    </div>

    <div>
      <h2>Cart</h2>
      <table>
        <thead>
          <tr>
            <th>Product</th>
            <th>Price</th>
            <th>Quantity</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in cart" :key="index">
            <td>{{ item.product.name }}</td>
            <td>₱{{ item.product.price }}</td>
            <td>
              <input type="number" v-model="item.quantity" @change="updateQuantity(item)">
            </td>
            <td><button class="remove-from-cart" @click="removeFromCart(index)">Remove</button></td>
          </tr>
        </tbody>
      </table>
      <p>Total: ₱{{ total }}</p>
    </div>

  </div>
</template>

<script>
export default {
name: 'ShoppingCart',
data() {
  return {
    products: [
      { id: 1, name: 'Milk', price: 135 },
      { id: 2, name: 'Egg', price:72 },
      { id: 3, name: 'Cheese', price: 63 },
      { id: 4, name: 'Yogurt', price: 25 },
      { id: 5, name: 'Pasta', price: 75 },
      { id: 6, name: 'Rice', price: 52 },
      { id: 7, name: 'Bread', price: 65 },
      { id: 8, name: 'Oats', price: 89 },
      { id: 9, name: 'Pork', price: 175 },
      { id: 10, name: 'Beef', price: 203 }
    ],
    cart: [],
    total: 0
  };
},
methods: {
  addToCart(product) {
  let found = this.cart.find(item => item.product.id === product.id);
  if (found) {
    found.quantity++;
  } else {
    this.cart.push({ product: product, quantity: 1, total: product.price });
  }
  this.calculateTotal();
},

  removeFromCart(index) {
    this.cart.splice(index, 1);
    this.calculateTotal();
  },
  updateQuantity(item) {
  item.total = item.product.price * item.quantity;
  this.calculateTotal();
},
calculateTotal() {
  let total = 0;
  this.cart.forEach(item => {
    item.total = item.product.price * item.quantity;
    total += item.total;
  });
  this.total = total;
}

}
};
</script>


<style scoped>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f4f4f4;
}

#app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1, h2 {
  text-align: center;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  padding: 10px;
  margin-bottom: 5px;
  background-color: #f9f9f9;
  border-radius: 4px;
}

button {
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button.add-to-cart {
  background-color: #cbc3e3;
  color: #000;
}

button.remove-from-cart {
  background-color: #dc143c;
  color: #fff;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th {
  padding: 8px;
  text-align: left;
  background-color: #cbc3e3;
  color: #000;
}

td {
  padding: 8px;
  text-align: left;
}

tbody tr:nth-child(even) {
  background-color: #f2f2f2;
}

input[type="number"] {
  width: 50px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

p {
  text-align: right;
  margin-top: 10px;
}
</style>
