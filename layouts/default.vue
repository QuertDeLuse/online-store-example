<template>
  <div>
    <CartMenu
      class="CartMenu"
      :class="{
        DisplayCartMenu: displayCartMenu,
        CartMenu_show: cartMenuShow,
        CartMenu_hide: cartMenuHide,
      }"
      @hideCartMenu="(cartMenuShow = false),(cartMenuHide = true)"
      @displayNoneCartMenu="(displayCartMenu = false), (cartMenuHide = false)"
    />
    <div class="navbar-wrapper">
      <div class="container">
        <div class="navbar">
          <CategoryMenu
            class="CategoryMenu"
            :class="{ CategoryMenu_show: categoryMenuShow }"
            @categoryOpened="categoryMenuShow = false"
          />
          <div class="menu-btn-wrapper">
            <a @click.prevent="toggleCategoryMenu" class="menu-btn"></a>
          </div>
          <div class="logo-wrapper">
            <h3 class="logo">TestList</h3>
          </div>
          <a @click.prevent="openCartMenu" href="#" class="cart-wrapper">
            <img class="cart" src="@/static/img/cart.png" alt="cart" />
            <div v-if="cartItems.length > 0" class="item-indicator-wrapper">
              <h3 class="item-indicator">{{ cartItems.length }}</h3>
            </div>
          </a>
        </div>
      </div>
    </div>

    <Nuxt
      class="backgoround"
      :class="[categoryMenuShow || cartMenuShow ? 'backgoround_hide' : '']"
    />
  </div>
</template>

<script>
import CategoryMenu from "@/components/categoryMenu";
import CartMenu from "@/components/cartMenu";

export default {
  async middleware({ store }) {
    if (store.getters.categories.length == 0) {
      await store.dispatch("fetchNewCategories");
    }
  },

  data() {
    return {
      categoryMenuShow: false,
      cartMenuShow: false,
      cartMenuHide: false,
      displayCartMenu: false,
    };
  },

  computed: {
    cartItems() {
      return this.$store.getters.cartItems;
    }
  },

  mounted() {
    this.$router.push("/" + this.$store.getters.activeCategory.name);
  },

  methods: {
    toggleCategoryMenu() {
      this.categoryMenuShow = !this.categoryMenuShow;
    },
    openCartMenu() {
      this.displayCartMenu = true;
      this.cartMenuShow = true;
      this.cartMenuHide = false;
    },
  },
};
</script>

<style lang="scss">
body {
  box-sizing: border-box;
  padding: 0px;
  margin: 0px;
  // overflow: hidden;
  // height: 100vh;

  font-family: "PT Sans", sans-serif;
}

.backgoround {
  transition: all 0.3s;
  opacity: 1;
}
.backgoround_hide {
  opacity: 0.1;
}

.container {
  max-width: 1260px;
  margin-left: auto;
  margin-right: auto;
  padding: 0px 20px;
}

.CartMenu {
  display: none;
  transition: all 0.3s;
}
.CartMenu_hide {
  animation: CartMenu_hide 0.3s;
}
.CartMenu_show {
  animation: CartMenu_show 0.3s;
}
.DisplayCartMenu {
  display: block;
}

.navbar-wrapper {
  position: fixed;
  z-index: 98;
  width: 100%;

  background-color: #fff;
  border-radius: 0px 0px 8px 8px;
  box-shadow: 0px 4px 16px 0px rgba(#000000, 0.05);

  .navbar {
    position: relative;
    height: 66px;
    display: flex;
    justify-content: space-between;
    align-items: center;

    .CategoryMenu {
      transition: all 0.3s;

      @media screen and (max-width: 1080px) {
        // display: none;
        left: -200px;
        opacity: 0;
      }
    }

    .CategoryMenu_show {
      @media screen and (max-width: 1080px) {
        // display: block;
        left: 0px;
        opacity: 1;
      }
    }

    .menu-btn-wrapper {
      display: none;

      .menu-btn {
        position: relative;
        width: 16px;
        height: 2px;

        &::before,
        &::after {
          content: "";
          position: absolute;
          width: 20px;
          height: 2px;
          background-color: #000;
        }
        &::before {
          top: -4px;
        }
        &::after {
          bottom: -4px;
        }

        transition: all 0.3s;
      }

      @media screen and (max-width: 1080px) {
        display: block;
      }
    }

    .logo-wrapper {
      .logo {
        font-weight: bold;
        font-size: 22px;
        margin: 0px;
      }
    }

    .cart-wrapper {
      cursor: pointer;
      position: relative;

      .cart {
      }

      .item-indicator-wrapper {
        position: absolute;
        top: 0px;
        right: 0px;

        width: 12px;
        height: 12px;
        background-color: #959dad;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 50%;

        .item-indicator {
          color: #fff;
          font-size: 8px;
        }
      }
    }
  }
}

@keyframes CartMenu_show {
  0% {
    opacity: 0;
    right: -500px;
  }
  100% {
    opacity: 1;
    right: 0px;
  }
}

@keyframes CartMenu_hide {
  0% {
    opacity: 1;
    right: 0px;
  }
  100% {
    opacity: 0;
    right: -500px;
  }
}
</style>