
<script setup lang="ts">
import { ref, computed } from 'vue';

interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
  image: string;
}

const products: Product[] = [
  { id: 1, name: 'Ноутбук Pro 14', price: 89990, category: 'Электроника', image: '💻' },
  { id: 2, name: 'Механическая клавиатура', price: 7500, category: 'Электроника', image: '⌨️' },
  { id: 3, name: 'Кружка разработчика', price: 690, category: 'Аксессуары', image: '☕' },
  { id: 4, name: 'Худи "Vue Master"', price: 3200, category: 'Одежда', image: '👕' },
  { id: 5, name: 'Мышь беспроводная', price: 2400, category: 'Электроника', image: '🖱️' },
  { id: 6, name: 'Стикерпак с логотипом', price: 150, category: 'Аксессуары', image: '🏷️' },
  { id: 7, name: 'Монитор 27"', price: 24990, category: 'Электроника', image: '🖥️' },
  { id: 8, name: 'Кепка "Frontend"', price: 1100, category: 'Одежда', image: '🧢' },
];


const searchQuery = ref('');
const sortBy = ref('default');
const cartList = ref<Product[]>([]); 


const displayProducts = computed(() => {
  const activeQuery = searchQuery.value.trim().toLowerCase();
  let result = products;
  if (activeQuery) {
    result = products.filter(product => 
      product.name.toLowerCase().includes(activeQuery)
    );
  }
  return [...result].sort((a, b) => {
    if (sortBy.value === 'price-asc') return a.price - b.price; 
    if (sortBy.value === 'price-desc') return b.price - a.price; 
    if (sortBy.value === 'name-asc') return a.name.localeCompare(b.name); 
    return 0; 
  });
});


const cartTotal = computed(() => {
  return cartList.value.reduce((sum, item) => sum + item.price, 0);
});


function addToCart(item: Product) {
  cartList.value.push(item);
}

function removeFromCart(index: number) {
  cartList.value.splice(index, 1);
}
</script>

<template>
  <div class="shop-container">

    <header class="controls">
      <input 
        v-model="searchQuery" 
        type="text" 
        placeholder="Поиск товаров по названию..."
        class="search-input"
      />
      
      <select v-model="sortBy" class="sort-dropdown">
        <option value="default">По умолчанию</option>
        <option value="price-asc">Цена: от меньшей к большей</option>
        <option value="price-desc">Цена: от большей к меньшей</option>
        <option value="name-asc">Название: А–Я</option>
      </select>
    </header>

    <main class="main-layout">
      <section class="products-grid">
        <div v-if="displayProducts.length === 0" class="no-results">
          Товары не найдены
        </div>
        
        <div v-for="product in displayProducts" :key="product.id" class="product-card">
          <div class="product-image">{{ product.image }}</div>
          <h3 class="product-name">{{ product.name }}</h3>
          <p class="product-category">{{ product.category }}</p>
          <div class="product-footer">
            <span class="product-price">{{ product.price.toLocaleString() }} ₽</span>
            <button @click="addToCart(product)" class="btn-add">В корзину</button>
          </div>
        </div>
      </section>

      <aside class="cart-sidebar">
        <h2>Корзина ({{ cartList.length }})</h2>
        
        <div v-if="cartList.length === 0" class="empty-cart">
          Корзина пуста
        </div>
        
        <ul v-else class="cart-items">
          <button @click="cartList = []" class="btn-clear">❌Очистить корзину</button>
          <li v-for="(item, index) in cartList" :key="index" class="cart-item">
            <span>{{ item.name }}</span>
            <div class="cart-item-controls">
              <span class="cart-item-price">{{ item.price.toLocaleString() }} ₽</span>
              <button @click="removeFromCart(index)" class="btn-remove" title="Удалить">&times;</button>
            </div>
          </li>
        </ul>
        
        <div v-if="cartList.length > 0" class="cart-summary">
          <strong>Итого:</strong>
          <span class="total-price">{{ cartTotal.toLocaleString() }} ₽</span>
        </div>
      </aside>
    </main>
  </div>
</template>

<style scoped>
.shop-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: sans-serif;
}

.controls {
  display: flex;
  gap: 15px;
  margin-bottom: 30px;
}

.search-input {
  flex: 1;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 6px;
}

.sort-dropdown {
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 6px;
  background: white;
}

.main-layout {
  display: grid;
  grid-template-columns: 3fr 1fr;
  gap: 30px;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 20px;
  align-content: start;
}

.no-results {
  grid-column: 1 / -1;
  text-align: center;
  color: #777;
  padding: 40px;
}

.product-card {
  border: 1px solid #eee;
  border-radius: 8px;
  padding: 15px;
  display: flex;
  flex-direction: column;
  background: #fbfbfb;
}

.product-image {
  font-size: 48px;
  text-align: center;
  margin-bottom: 15px;
}

.product-name {
  margin: 0 0 5px 0;
  font-size: 18px;
}

.product-category {
  font-size: 14px;
  color: #888;
  margin: 0 0 15px 0;
}

.product-footer {
  margin-top: auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.product-price {
  font-weight: bold;
  font-size: 16px;
}

.btn-add {
  background: #42b883;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
}

.btn-add:hover {
  background: #33a06f;
}

.cart-sidebar {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  background: #fff;
  align-self: start;
}

.empty-cart {
  color: #999;
  text-align: center;
  padding: 20px 0;
}

.cart-items {
  list-style: none;
  padding: 0;
  margin: 0 0 20px 0;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid #eee;
  font-size: 14px;
}

.cart-item-controls {
  display: flex;
  align-items: center;
  gap: 10px;
}

.cart-item-price {
  font-weight: bold;
}

.btn-remove {
  background: none;
  border: none;
  color: #ff4d4f;
  font-size: 18px;
  cursor: pointer;
  padding: 0 5px;
}

.cart-summary {
  display: flex;
  justify-content: space-between;
  border-top: 2px solid #ddd;
  padding-top: 15px;
  font-size: 18px;
}

.total-price {
  color: #42b883;
  font-weight: bold;
}
</style>
