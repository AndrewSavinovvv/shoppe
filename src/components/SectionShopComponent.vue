<script setup>
import item from "@/assets/img/Lira Earrings.png";
import { ref, computed } from 'vue';
import SelectComponent from "@/components/SelectComponent.vue";
import SearchComponent from "@/components/SearchComponent.vue";

const items = [
  { title: "Lira Earrings", price: 20, img: item, sale: "-21%", collection: "Earrings" },
  { title: "Hal Earrings", price: 25, img: item, collection: "Earrings" },
  { title: "Kaede Hair Pin Set Of 3", price: 30, img: item, collection: "Earrings" },
  { title: "Hair Pin Set of 3", price: 30, img: item, collection: "Necklaces" },
  { title: "Plaine Necklace", price: 19, img: item, stock: "Sold out", collection: "Necklaces" },
  { title: "Yuki Hair Pin Set of 3", price: 29, img: item, collection: "Necklaces" },
];

const searchQuery = ref('');

const filteredItems = computed(() => {
  let result = items;
  if (selectedCollection.value !== "All") {
    result = result.filter(item => item.collection === selectedCollection.value);
  }
  if (searchQuery.value) {
    result = result.filter(item => item.title.toLowerCase().includes(searchQuery.value.toLowerCase()));
  }
  if (isActiveSale.value) {
    result = result.filter(item => item.sale);
  }
  if (isActiveStock.value) {
    result = result.filter(item => !item.stock || item.stock !== "Sold out");
  }
  return result;
});

const isActiveSale = ref(false);
const isActiveStock = ref(false);
const toggleSaleFilter = () => {
  isActiveSale.value = !isActiveSale.value;
};

const toggleStockFilter = () => {
  isActiveStock.value = !isActiveStock.value;
};

const sortOptions = ref([
  { label: "Price: Low to High", value: "PriceLowToHigh" },
  { label: "Price: High to Low", value: "PriceHighToLow" },
  { label: "Default", value: "Default" },
]);

const collectionOptions = ref([
  { label: "Earrings", value: "Earrings" },
  { label: "Necklaces", value: "Necklaces" },
  { label: "All", value: "All" },
]);

const selectedSort = ref("Default");
const selectedCollection = ref("All");

const sortedItems = computed(() => {
  let itemsToSort = filteredItems.value.slice();
  if (selectedSort.value === "PriceLowToHigh") {
    return itemsToSort.sort((a, b) => a.price - b.price);
  } else if (selectedSort.value === "PriceHighToLow") {
    return itemsToSort.sort((a, b) => b.price - a.price);
  }
  return itemsToSort;
});

const sortItems = (sortType) => {
  selectedSort.value = sortType;
};

const filterByCollection = (collection) => {
  selectedCollection.value = collection;
};
</script>

<template>
  <h1 class="shop__title container">Shop The Latest</h1>
  <div class="shop container">
    <div class="shop__left">
      <div class="shop__left__list">
        <ul class="shop__left__list___item">
          <li class="shop__left__list___item__search">
            <SearchComponent v-model="searchQuery" />
            <img class="search__icon" src="@/assets/icon/search.svg" alt="" />
          </li>
          <SelectComponent :options="collectionOptions" propNames="Shop By" @select="filterByCollection" />
          <SelectComponent :options="sortOptions" propNames="Sort By" @select="sortItems" />

          <li class="shop__left__btn">
            <span>On sale</span>
            <button
                :class="['toggle__button', { active: isActiveSale }]"
                @click="toggleSaleFilter"
            ></button>
          </li>
          <li class="shop__left__btn">
            <span>In stock</span>
            <button
                :class="['toggle__button', { active: isActiveStock }]"
                @click="toggleStockFilter"
            ></button>
          </li>
        </ul>
      </div>
    </div>
    <div class="shop__right">
      <div v-for="item in sortedItems" class="shop__right__item" :key="item.title">
        <div class="item__img">
          <img :src="item.img" alt="" />
          <span class="item__sale" v-if="item.sale">{{ item.sale }}</span>
          <span class="item__sale" v-if="item.stock">{{ item.stock }}</span>
        </div>
        <span class="item__title">{{ item.title }}</span>
        <span class="item__price">${{ item.price }}</span>
      </div>
    </div>
  </div>
</template>
<style scoped>
.shop {
  display: flex;
  justify-content: space-between;
}
.shop__title {
  margin-top: 93px;
  font-family: DM Sans, sans-serif;
  font-size: 33px;
  font-weight: 500;
  color: #000000;
  margin-bottom: 39px;
}
.shop__right {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.shop__right__item {
  display: flex;
  flex-direction: column;
  align-items: start;
}
.item__price {
  font-size: 20px;
  font-family: DM Sans, sans-serif;
  font-weight: 500;
  color: #A18A68;
  margin-top: 16px;
}
.item__title {
  font-size: 20px;
  font-family: DM Sans, sans-serif;
  color:#000000 ;
  font-weight: 400;
  margin-top: 24px;
}
.item__img{
  position: relative;
}
.item__sale {
  position: absolute;
  top: 16px;
  left: 16px;
  padding: 2px 8px;
  background: #A18A68;
  color: white;
  border-radius: 5px;
  font-size: 12px;
  font-family: "DM Sans", sans-serif;
  font-weight: 400;
}
.shop__left__list___item {
  list-style: none;
  padding: 0;
}
.shop__left__list___item__search {
  display: flex;
  position: relative;
  margin-bottom: 39px;
}
.shop__left {
  width: 261px;
}

.search__icon {
  position: absolute;
  top: 5px;
  right: 0;
  pointer-events: none;
}

.toggle__button {
  width: 33px;
  height: 20px;
  border: none;
  border-radius: 15px;
  background-color: #707070;
  position: relative;
  cursor: pointer;
  transition: background-color 0.3s;
}
.shop__left__btn {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 50px;
}
.toggle__button::before {
  content: '';
  width: 12px;
  height: 12px;
  background-color: white;
  border-radius: 50%;
  position: absolute;
  top:4px;
  left: 5px;
  transition: transform 0.3s;
}

.toggle__button:active {
  background-color: #707070;
}

.toggle__button.active {
  background-color: #000000;
}

.toggle__button.active::before {
  transform: translateX(12px);
}
.shop__left__btn span {
  font-size: 16px;
  color: #000000;
  font-family: "DM Sans", sans-serif;
  font-weight: 400;
}

@media (max-width: 1258px) {
  .shop__title, .shop__left,.item__sale {
    display: none;
  }
  .shop__right {
    grid-template-columns: repeat(2, 1fr);
    gap: 16px;
    overflow-x: hidden;
    margin-top: 16px;
  }

  .shop__right__item {
    max-width: 100%;
    box-sizing: border-box;
  }

  .item__img img {
    width: 100%;
    height: auto;
  }

  .category-filters span {
    cursor: pointer;
    padding: 8px 12px;
    border-radius: 5px;
    border: 1px solid #D8D8D8;
    width: 100%;
    text-align: center;
  }
  .category-filters span:nth-child(1) {
    margin-right: 8px;
  }
  .category-filters span:nth-child(2) {
    margin-left: 8px;
  }
  .category-filters span:hover {
    background-color: white;
    color: #000000;

  }
  .shop {
    display: flex;
    flex-direction: column;
  }
}
</style>