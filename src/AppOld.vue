<script setup lang="ts">
import { ref, computed} from 'vue';

const products = [
  { id: 1, name: 'Ноутбук Pro 14', price: 89990, category: 'Электроника', image: '💻' },
  { id: 2, name: 'Механическая клавиатура', price: 7500, category: 'Электроника', image: '⌨️' },
  { id: 3, name: 'Кружка разработчика', price: 690, category: 'Аксессуары', image: '☕' },
  { id: 4, name: 'Худи "Vue Master"', price: 3200, category: 'Одежда', image: '👕' },
  { id: 5, name: 'Мышь беспроводная', price: 2400, category: 'Электроника', image: '🖱️' },
  { id: 6, name: 'Стикерпак с логотипом', price: 150, category: 'Аксессуары', image: '🏷️' },
  { id: 7, name: 'Монитор 27"', price: 24990, category: 'Электроника', image: '🖥️' },
  { id: 8, name: 'Кепка "Frontend"', price: 1100, category: 'Одежда', image: '🧢' },
];

const SearchQuery = ref('');
const sortBy = ref('default');

const DisplayProducts = computed(() => {
  let result = products;
  const ActiveQuery = SearchQuery.value.trim().toLowerCase();
  if (SearchQuery.value) result = products.filter(product => 
    product.name.toLowerCase().includes(ActiveQuery)
  );
  return [...result].sort((a, b) => {
    if (sortBy.value === 'price-asc') {
      return a.price - b.price; 
    }
    if (sortBy.value === 'price-desc') {
      return b.price - a.price; 
    }
    if (sortBy.value === 'name-asc') {
      return a.name.localeCompare(b.name); 
    }
    return 0; 
  });

});

let CartList = []

function AddToCart (item) {
  CartList.push(item)
}
</script>

<template>
  <input 
      v-model="SearchQuery" 
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
  <div class="product-grid-container">
    <div 
      v-for="p in DisplayProducts" 
      :key="p.id" 
      class="product-grid-item"
    >
      <h3>{{ p.image }}</h3>
      <p>{{ p.name }}</p>
      <button @click="AddToCart(p)">BUY {{p.price}} RUB</button>
    </div>
  </div>
  <div v-for="p in CartList">
    <p>{{ p.name }}</p>
  </div>
</template>



<style scoped>
  .product-grid-container {
    display: grid;
    /* Crucial properties for responsive auto-wrapping: */
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 16px; /* Spacing between grid cells */
    width: 100%;
  }

  .product-grid-item {
    background-color: #f4f4f4;
    padding: 20px;
    border-radius: 8px;
    border: 1px solid #ddd;
  }
</style>
