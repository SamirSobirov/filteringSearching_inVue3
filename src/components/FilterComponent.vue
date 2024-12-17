<template>
  <div class="filter-component">
    <div class="box_filter">
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Поиск..."
        class="search-input"
      />

      <select v-model="selectedCategory" class="category-select">
        <option value="">Все категории</option>
        <option
          v-for="(category, index) in uniqueCategories"
          :key="index"
          :value="category"
        >
          {{ category }}
        </option>
      </select>
    </div>

    <transition-group name="list" tag="ul" class="item-list">
      <li v-for="item in filteredItems" :key="item.id" class="item">
        {{ item.name }} - {{ item.category }}
      </li>
    </transition-group>

    <p v-if="filteredItems.length === 0" class="no-results">
      Нет товаров, соответствующих вашим фильтрам.
    </p>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const items = [
  { id: 1, name: "Огурец", category: "Овощи" },
  { id: 2, name: "Яблоко", category: "Фрукты" },
  { id: 3, name: "Морковь", category: "Овощи" },
  { id: 4, name: "Банан", category: "Фрукты" },
  { id: 5, name: "Брокколи", category: "Овощи" },
  { id: 6, name: "Киви", category: "Фрукты" },
  { id: 7, name: "Картошка", category: "Овощи" },
  { id: 8, name: "Мандарин", category: "Фрукты" },
  { id: 9, name: "Тыква", category: "Овощи" },
  { id: 10, name: "Арбуз", category: "Фрукты" },
  { id: 11, name: "Лук", category: "Овощи" },
  { id: 12, name: "Какос", category: "Фрукты" },
  { id: 13, name: "Капуста", category: "Овощи" },
];

const searchQuery = ref("");
const selectedCategory = ref("");

const uniqueCategories = computed(() => {
  const categories = items.map((item) => item.category);
  return [...new Set(categories)];
});

const filteredItems = computed(() => {
  return items.filter((item) => {
    const matchesText = item.name
      .toLowerCase()
      .includes(searchQuery.value.toLowerCase());
    const matchesCategory =
      selectedCategory.value === "" || item.category === selectedCategory.value;
    return matchesText && matchesCategory;
  });
});
</script>

<style scoped></style>
