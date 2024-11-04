<script setup>

</script>

<template>
  <div class="header container">
    <h2 class="header__title"><span>S</span>HOPPE</h2>
   <div class="header__mob">
     <div class="header__mob__cart">
       <img class="header__mob__img" src="@/assets/icon/cart.svg" alt="">
       <span class="cart-count">1</span>
     </div>
     <div   class="burger-menu">
       <span class="burger-line"></span>
       <span class="burger-line"></span>
       <span class="burger-line short-line"></span>
     </div>

   </div>
    <div class="header__menu">
      <ul class="header__menu__list">
        <li class="active header__link">Shop</li>
        <li class="header__link">Blog</li>
        <li class="header__link">Our Story</li>
        <li class="header__menu__list__line"></li>
        <li>
          <img src="@/assets/icon/search.svg" alt="">
        </li>
        <li>
          <img src="@/assets/icon/cart.svg" alt="">
        </li>
        <li>
          <img src="@/assets/icon/user.svg" alt="">
        </li>
      </ul>
    </div>
  </div>
  <span class="header__footer__line container"></span>
 <div class="search">
   <img class="search__icon" src="@/assets/icon/search.svg" alt="">
   <input class="header__search" type="search" placeholder="Search" />
 </div>
</template>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  padding-top: 49px;
}
.header__title {
  font-family: "Allerta Stencil", sans-serif;
  font-size: 35px;
  font-weight: 400;
  line-height: 40px;
  color: #000000;
}
.header__title span {
  color: #A18A68;
}
.header__menu__list {
  list-style-type: none;
  display: flex;
  justify-content: space-between;
  font-size: 16px;
  font-family: DM Sans, sans-serif;
  color: #000000;
}
.header__menu__list {
  list-style-type: none;
  display: flex;
  gap: 39px;
  align-items: center;
}

.header__menu__list__line {
  content: '';
  display: inline-block;
  width: 1px;
  height: 17px;
  background-color:#707070;
  margin-right: 16px;
}
.header__footer__line {
  display: block;
  width: 100%;
  height: 1px;
  background-color: #D8D8D8;
  margin-top: 17px;
}
.header__menu__list .active {
  position: relative;
}

.header__menu__list .active::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: -38px;
  width: 100%;
  height: 2px;
  background-color: #000000;
}
.header__link {
  margin-right: 64px;
}
.header__link:nth-child(3) {
  margin-right: 48px;
}
.burger-menu {
  display: none;
  flex-direction: column;
  gap: 4px;
  cursor: pointer;
}

.burger-line {
  width: 24px;
  height: 2px;
  background-color: #000000;
}
.short-line {
  width: 16px;
  height: 2px;
  transform: translateX(50%);
}
.header__mob__cart {
  display: none;
}
.cart-count {
  display: none;
}
.header__search {
  display: none
}
.search__icon {
  display: none;
}
@media (max-width: 808px) {
  .header {
    align-items: center;
    justify-content: space-between;
    margin:0;
    padding: 0;
  }

  .header__title {
    align-self: flex-start;
  }

  .header__mob__cart {
    position: relative;
    display: flex;
   margin-right: 16px;
  }
  .header__menu,.header__footer__line {
    display: none;
  }

  .burger-menu {
    display: flex;
    flex-direction: column;
    gap: 4px;
    cursor: pointer;
  }
  .search {
    margin-top: 16px;
  }
  .search,.header__search {
    display: flex;
    width: 100%;
    background: #EFEFEF;
    border: none;
    border-radius: 15px;
    padding: 6px 6px;
    align-items: center;
    color:#707070 ;
  }
  .cart-count {
    display: flex;
    align-items: center;
    position: absolute;
    font-size: 7px;
    color: #000000;
    top: 0;
    right: 0;
    border: 1px solid black;
    border-radius: 50px;
    height: 8px;
    width: 8px;
  }
  .header__mob {
    display: flex;
    align-items: center;
  }
  .search__icon {
    display: flex;
  }
}
</style>