<template>
  <div class="filter-component">
    <input
      v-model="searchQuery"
      type="text"
      placeholder="Search..."
      class="search-input"
    />

    <select v-model="selectedCategory" class="category-select">
      <option value="">All Categories</option>
      <option
        v-for="(category, index) in uniqueCategories"
        :key="index"
        :value="category"
      >
        {{ category }}
      </option>
    </select>

    <transition-group name="list" tag="ul" class="item-list">
      <li v-for="item in filteredItems" :key="item.id" class="item">
        {{ item.name }} - {{ item.category }}
      </li>
    </transition-group>

    <p v-if="filteredItems.length === 0" class="no-results">
      No items match your filters.
    </p>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const items = [
  { id: 1, name: "Apple", category: "Fruits" },
  { id: 2, name: "Carrot", category: "Vegetables" },
  { id: 3, name: "Banana", category: "Fruits" },
  { id: 4, name: "Broccoli", category: "Vegetables" },
  { id: 5, name: "Orange", category: "Fruits" },
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
