<template>
  <main class="content container">
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" :to="{ name: 'main' }">
            Каталог
          </router-link>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link">
            Корзина
          </a>
        </li>
      </ul>

      <h1 class="content__title">
        Корзина
      </h1>
      <span class="content__info">
        {{ totalQuantity }} товара
      </span>
    </div>

    <section class="cart">
      <form class="cart__form form" action="#" method="POST">
        <div class="cart__field">

          <div v-if="isLoading" >
            <img src="/img/1496.gif" style="margin: 0 auto; display: block;" width="64" alt="loading">
          </div>

          <ul class="cart__list" v-if="!isLoading">
            <CartItem v-for="item in products" :key="item.product.id" :item="item" />
          </ul>
        </div>

        <div class="cart__block">
          <p class="cart__desc">
            Мы&nbsp;посчитаем стоимость доставки на&nbsp;следующем этапе
          </p>
          <p class="cart__price">
            Итого: <span> {{totalPrice | numberFormat}} ₽</span>
          </p>

          <router-link tag="button" :to="{name: 'order'}" class="cart__button button button--primery" type="submit">
            Оформить заказ
          </router-link>
        </div>
      </form>
    </section>
  </main>
</template>

<script>
  import numberFormat from "@/helpers/numberFormat";
  import { mapGetters } from "vuex";
  import { mapActions } from "vuex";
  import CartItem from "@/components/CartItem.vue";

  export default {
    data() {
      return {
        isLoading: true
      };
    },
    filters: {
      numberFormat,
    },
    components: {
      CartItem
    },
    computed: {
      ...mapGetters({products: 'cartDetailProducts', totalPrice: 'cartTotalPrice', totalQuantity: 'cartQuantity'})
    },
    methods: {
      ...mapActions(['loadCart']),

      loadProducts() {
        this.isLoading = true;
        clearTimeout(this.loadProductsTimer);
        this.loadProductsTimer = setTimeout(() => {
          this.loadCart()
              .then(this.isLoading = false);
        }, 1500)
      }
    },
    watch: {
      page() {
        this.loadProducts();
      }
  },
    created() {
      this.loadProducts();
    }
  }
</script>