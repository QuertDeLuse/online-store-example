<template>
  <div class="card-category-wrapper">
    <div class="card-category">
      <div class="card-category__img-wrapper">
        <img
          :src="'http://front-test.idalite.com' + product.photo"
          alt="product-img"
          class="card-category__img"
        />
      </div>

      <p class="card-category__name">{{ product.name }}</p>
      <h3 class="card-category__price">{{ product.price }} ₽</h3>

      <div class="card-category__rating">
        <img src="@/static/img/star.png" alt="star" class="rating__img" />
        <h3 class="rating__number">{{ product.rating }}</h3>
      </div>
      <a
        @click.prevent="toggleToCart"
        href="#"
        :class="{
          'card-category__cart-img-wrapper_added': check(),
        }"
        class="card-category__cart-img-wrapper"
      >
        <img
          src="@/static/img/cart.png"
          alt="cart"
          class="card-category__cart-img"
        />
      </a>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    product: Object,
  },

  methods: {
    toggleToCart() {
      if (this.check()) {
        let newCartItems = this.cartItems.filter((item) => {
          return item.id != this.product.id;
        });
        this.$store.dispatch("changeAllCartItems", newCartItems);
      } else {
        this.$store.dispatch("addCartItem", this.product);
      }

      let cartItemsToLocalStorage = [];
      this.cartItems.forEach((item, key) => {
        let itemToLS = `${item.id}:${item.name}:${item.price}:${item.rating}:${item.photo}`;
        cartItemsToLocalStorage.push(itemToLS);
      });
      localStorage.setItem("cartItemsArray", cartItemsToLocalStorage);
    },

    check() {
      let contain = false;
      this.cartItems.forEach((item) => {
        if (this.product.id == item.id) contain = true;
      });
      return contain;
    },
  },

  computed: {
    cartItems() {
      return this.$store.getters.cartItems;
    },
  },
};
</script>

<style lang="scss">
.card-category-wrapper {
  box-sizing: border-box;
  display: block;
  width: 260px;
  padding: 16px;
  background: #fff;
  box-shadow: 0px 4px 16px rgba(0, 0, 0, 0.05);
  border-radius: 8px;
  margin: 0px 8px 16px 8px;

  .card-category {
    display: block;
    position: relative;

    .card-category__img-wrapper {
      display: flex;
      justify-content: center;
      height: 180px;
      .card-category__img {
        object-fit: cover;
        max-width: 100%;
        height: 100%;
      }
    }

    .card-category__name {
      margin: 0px;
      margin-top: 16px;
      font-size: 14px;
      color: #59606d;
    }
    .card-category__price {
      margin: 0px;
      margin-top: 6px;
      font-weight: bold;
      font-size: 14px;
      line-height: 18px;
      color: #1f1f1f;
    }

    .card-category__rating {
      position: absolute;
      top: 0px;
      left: 0px;
      display: flex;
      align-items: center;

      .rating__img {
      }
      .rating__number {
        margin: 0px;
        font-weight: bold;
        font-size: 10px;
        line-height: 13px;
        color: #f2c94c;
      }
    }

    .card-category__cart-img-wrapper {
      width: 16px;
      height: 16px;
      position: absolute;
      top: 0px;
      right: 0px;
      transition: all 0.3s;

      .card-category__cart-img {
        object-fit: cover;
        width: 100%;
        height: 100%;
        filter: brightness(5);
        opacity: 1;

        transition: all 0.3s;

        &:hover {
          filter: brightness(0);
        }
      }
    }

    .card-category__cart-img-wrapper_added {
      .card-category__cart-img {
        opacity: 0;
      }

      &::before {
        position: absolute;
        // bottom: -10px;
        content: "";
        width: 12px;
        height: 4px;
        background-color: transparent;
        border-bottom: 2px solid rgba(#1f1f1f, 0.5);
        border-left: 2px solid rgba(#1f1f1f, 0.5);
        transform: rotate(-45deg);
        transition: all 0.3s;
      }

      &:hover {
        &::before {
          border-bottom: 2px solid rgba(#1f1f1f, 1);
          border-left: 2px solid rgba(#1f1f1f, 1);
        }
      }
    }
  }

  @media screen and (max-width: 1080px) {
    width: 31%;
    margin: 1%;
  }

  @media screen and (max-width: 768px) {
    width: 48%;
    margin: 1%;
  }

  @media screen and (max-width: 500px) {
    width: 94%;
    margin: 3%;
  }
}
</style>