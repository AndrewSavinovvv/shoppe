<script setup>

</script>

<template>
  <div class="footer container">
    <div class="footer__line"></div>
    <div class="footer__nav">
      <ul class="footer__nav__list">
        <li class="footer__nav__list__item">CONTACT</li>
        <li class="footer__nav__list__item">TERMS OF SERVICES</li>
        <li class="footer__nav__list__item">SHIPPING AND RETURNS</li>
      </ul>
      <div class="footer__input">
        <input placeholder="Give an email, get the newsletter.">
        <img class="footer__input__img" src="@/assets/icon/arrow-right.svg" alt="">

      </div>
      <div class="footer__checkbox">
        <input type="checkbox" id="agree">
        <label for="agree">I agree to the website’s terms and conditions</label>
      </div>
    </div>
    <div class="footer__media">
      <p class="footer__media__text">
        © <span>2021 Shelly.</span> Terms of use <span>and</span> privacy policy.
      </p>
      <div>
        <ul class="footer__media__list">
          <li class="footer__media__list__text">Follow us</li>
          <li class="footer__media__list__line"></li>
          <li>
            <img src="@/assets/icon/in.svg" alt="">
          </li>
          <li>
            <img src="@/assets/icon/facebook.svg" alt="">
          </li>
          <li>
            <img src="@/assets/icon/inst.svg" alt="">
          </li>
          <li>
            <img src="@/assets/icon/twitter.svg" alt="">
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
.footer {
  margin-top: 252px;
}
.footer__line {
  height: 1px;
  background: #D8D8D8;
  margin-bottom: 55px;
}
.footer__nav__list {
  display: flex;
  list-style: none;
  gap: 41px;
  color: #707070;
  font-weight: 400;
  font-size: 16px;
  font-family: "DM Sans", sans-serif;
  padding: 0;
}
.footer__nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.footer__input {
  display: flex;
  align-items: center;
  border-bottom:1px solid #000000;
}

.footer__input input {
  padding: 8px;
  font-size: 16px;
  font-family: "DM Sans", sans-serif;
  border: none;
  outline: none;
}

.footer__input img {
  cursor: pointer;
  margin-left: 128px;
}
.footer__media {
  display: flex;
  justify-content: space-between;
}
.footer__media__text {
  font-size: 16px;
  font-weight: 400;
  font-family: "DM Sans", sans-serif;
  color: #707070;
}
.footer__media__text span {
  font-weight: bold;
}
.footer__media__list {
  list-style: none;
  display: flex;
}
.footer__media__list li {
  padding-left: 30px;
}
.footer__media {
  margin-top: 48px;
}
.footer__checkbox {
  display: none;
}

@media (max-width: 1258px) {
  .footer {
    display: flex;
    flex-direction: column;
  }

  .footer__nav, .footer__nav__list {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  .footer__input {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }

  .footer__checkbox {
    display: flex;
    margin-top: 10px;
  }

  .footer__nav__list {
    order: 1;
    margin-bottom: 20px;
  }

  .footer__input {
    order: 0;
    display: flex;
    align-items: flex-start;
    position: relative;
  }

  .footer__media {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    order: 2;
    margin-top: 20px;
  }

  .footer__media__text {
    order: 3;
    font-size: 14px;
    margin-top: 10px;
  }

  .footer__media__list {
    display: flex;
    gap: 20px;
    order: 2;
    padding: 0;
  }

  .footer__line {
    display: none;
  }
  .footer__input__img {
    position: absolute;
    top: 13px;
    right: -90px;
  }
  .footer__input input {
    width:100%
  }
  .footer__checkbox {
    border-bottom: none;
    width: 100%;
  }
  .footer__media__list li:nth-child(3) {
    display: none;
  }
  .footer__media__list li {
    display: flex;
    align-items: flex-start;
    padding: 0;
  }
  .footer__media__list__line{
    width: 47px;
    height: 1px;
    background: #000000;
  }
  .footer__media__list__text {
    font-size: 12px;
    color: #000000;
    font-family: "DM Sans", sans-serif;
    font-weight: 400;
  }
  .footer__media__list {
    align-items: center;
  }
}
</style>