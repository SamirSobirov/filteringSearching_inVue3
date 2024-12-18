<template>
  <div class="filter">
    <div class="filter__controls">
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Поиск..."
        class="filter__input"
      />

      <select v-model="selectedCategory" class="filter__select">
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

    <transition-group name="filter__list" tag="ul" class="filter__list">
      <li v-for="item in filteredItems" :key="item.id" class="filter__item">
        {{ item.name }} - {{ item.category }}
      </li>
    </transition-group>

    <p v-if="filteredItems.length === 0" class="filter__message">
      Нет товаров!! Кыш :)
    </p>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
interface Item {
  id: number;
  name: string;
  category: string;
}

const items: Item[] = [
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
  { id: 12, name: "Кокос", category: "Фрукты" },
  { id: 13, name: "Капуста", category: "Овощи" },
];

const searchQuery = ref<string>("");
const selectedCategory = ref<string>("");

const uniqueCategories = computed<string[]>(() => {
  const categories = items.map((item) => item.category);
  return [...new Set(categories)];
});

const filteredItems = computed<Item[]>(() => {
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
