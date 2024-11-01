<script setup>
import item from "@/assets/img/Lira Earrings.png";
import { ref, computed } from 'vue';

const items = [
  { title: "Lira Earrings", price: "$20.00", img: item, sale: "-21%" },
  { title: "Hal Earrings", price: "$25.00", img: item },
  { title: "Kaede Hair Pin Set Of 3", price: "$30.00", img: item },
  { title: "Hair Pin Set of 3", price: "$30.00", img: item },
  { title: "Plaine Necklace", price: "$19.00", img: item, stock: "Sold out" },
  { title: "Yuki Hair Pin Set of 3", price: "$29.00", img: item },
];

const isActiveSale = ref(false);
const isActiveStock = ref(false);
const showShopByMenu = ref(false);
const showSortByMenu = ref(false);
const selectedCategory = ref("All");
const selectedSort = ref("Default");

const minPrice = ref(0);
const maxPrice = ref(180);
const searchQuery = ref("");

const filteredItems = computed(() => {
  return items.filter(item => {
    const priceValue = parseFloat(item.price.replace(/[^0-9.-]+/g, ""));
    const matchesPrice = priceValue >= minPrice.value && priceValue <= maxPrice.value;
    const matchesSearch = item.title.toLowerCase().includes(searchQuery.value.toLowerCase());
    const matchesSale = !isActiveSale.value || item.sale;
    const matchesStock = !isActiveStock.value || (!item.stock || item.stock !== "Sold out");
    return matchesPrice && matchesSearch && matchesSale && matchesStock;
  });
});

const sortedItems = computed(() => {
  let itemsToSort = filteredItems.value;
  if (selectedSort.value === "PriceLowToHigh") {
    return itemsToSort.sort((a, b) => parseFloat(a.price.replace(/[^0-9.-]+/g, "")) - parseFloat(b.price.replace(/[^0-9.-]+/g, "")));
  } else if (selectedSort.value === "PriceHighToLow") {
    return itemsToSort.sort((a, b) => parseFloat(b.price.replace(/[^0-9.-]+/g, "")) - parseFloat(a.price.replace(/[^0-9.-]+/g, "")));
  }
  return itemsToSort;
});

const toggleSaleFilter = () => {
  isActiveSale.value = !isActiveSale.value;
};

const toggleStockFilter = () => {
  isActiveStock.value = !isActiveStock.value;
};

const toggleShopByMenu = () => {
  showShopByMenu.value = !showShopByMenu.value;
};

const toggleSortByMenu = () => {
  showSortByMenu.value = !showSortByMenu.value;
};

const filterByCategory = (category) => {
  selectedCategory.value = category;
  showShopByMenu.value = false;
};

const sortItems = (sortType) => {
  selectedSort.value = sortType;
  showSortByMenu.value = false;
};

</script>

<template>
  <h1 class="shop__title container">Shop The Latest</h1>
  <div class="shop container">
    <div class="shop__left">
      <div class="shop__left__list">
        <ul class="shop__left__list___item">
          <li class="shop__left__list___item__search">
            <input
                class="shop__left__search"
                placeholder="Search..."
                type="text"
                v-model="searchQuery"
            />
            <img class="search__icon" src="@/assets/icon/search.svg" alt="" />
          </li>
          <li class="shop__left__sort">
            <span @click="toggleShopByMenu">Shop By</span>
            <span>
              <img src="@/assets/icon/arrow-down.svg" alt="" />
            </span>
            <ul v-if="showShopByMenu" class="dropdown__menu">
              <li @click="filterByCategory('Earrings')">Earrings</li>
              <li @click="filterByCategory('Necklaces')">Necklaces</li>
              <li @click="filterByCategory('All')">All</li>
            </ul>
          </li>
          <li class="shop__left__shop">
            <span @click="toggleSortByMenu">Sort By</span>
            <span>
              <img src="@/assets/icon/arrow-down.svg" alt="" />
            </span>
            <ul v-if="showSortByMenu" class="dropdown__menu">
              <li @click="sortItems('PriceLowToHigh')">Price: Low to High</li>
              <li @click="sortItems('PriceHighToLow')">Price: High to Low</li>
              <li @click="sortItems('Default')">Default</li>
            </ul>
          </li>
          <li class="shop__left__filter">
            <div class="price__filter">
              <input type="range" min="0" max="180" step="1" v-model="minPrice" />
            </div>
            <p class="filter__text">Price: ${{ minPrice }} - ${{ maxPrice }}
              <span>Filter</span>
            </p>
          </li>
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
    <div class="category-filters">
      <span>Earring</span>
      <span>Necklace</span>
    </div>
    <div class="shop__right">
      <div v-for="item in sortedItems" class="shop__right__item" :key="item.title">
        <div class="item__img">
          <img :src="item.img" alt="" />
          <span class="item__sale" v-if="item.sale">{{ item.sale }}</span>
          <span class="item__sale" v-if="item.stock">{{ item.stock }}</span>
        </div>
        <span class="item__title">{{ item.title }}</span>
        <span class="item__price">{{ item.price }}</span>
      </div>
    </div>
  </div>
</template>
<style scoped>
.shop__left__sort,.shop__left__shop {
  position: relative;
}
.dropdown__menu {
  width: 215px;
  position: absolute;
  background: white;
  top: 0;

  border: 1px solid #ccc;
  z-index: 1000;
  list-style: none;
}
.price__filter input[type="range"] {
  -webkit-appearance: none;
  width: 100%;
  background: transparent;
  height: 4px;
}

.price__filter input[type="range"]::-webkit-slider-runnable-track {
  background: #000000;
  height: 2px;
  border-radius: 2px;
}

.price__filter input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 2px;
  height: 10px;
  background: #000000;
  cursor: pointer;
  margin-top: -4px;
}

.dropdown__menu li {
  padding: 5px;
  cursor: pointer;
}

.dropdown__menu li:hover {
  background-color: #f0f0f0;
}
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
.shop__left__search {
  padding: 11px 0;
  border: none;
  border-bottom: 1px solid  #D8D8D8;
  font-size: 14px;
  font-family: "DM Sans", sans-serif;
  color: #707070;
  width: 261px;
}
.search__icon {
  position: absolute;
  top: 5px;
  right: 0;
  pointer-events: none;
}
.shop__left__sort,.shop__left__shop {
  border: 1px solid #D8D8D8;
  padding: 12px 15px;
  border-radius: 5px;
  width: 240px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 16px;
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
  transform: translateX(30px);
}
.shop__left__btn span {
  font-size: 16px;
  color: #000000;
  font-family: "DM Sans", sans-serif;
  font-weight: 400;
}
.price-filter input[type="range"] {
  -webkit-appearance: none;
  width: 100%;
  background: transparent;
  height: 4px;
}

.price-filter input[type="range"]::-webkit-slider-runnable-track {
  background: #000000;
  height: 2px;
  border-radius: 2px;
}


.price-filter input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 2px;
  height: 10px;
  background: #000000;
  cursor: pointer;
  margin-top: -4px;
}

.filter__text {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #707070;
  font-family: "DM Sans", sans-serif;
  font-size: 14px;
}
.filter__text span {
  color: #A18A68;
  font-family: "DM Sans", sans-serif;
  font-size: 14px;
  font-weight: 400;
}
.category-filters {
  display: none;
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
  .category-filters {
    display: flex;
    justify-content: space-around;
    padding-top: 10px;
    background-color: white;
    font-family: "DM Sans", sans-serif;
    font-size: 16px;
    font-weight: 500;
    color: #000000;

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