<template>
  <li class="cart__item product">
    <div class="product__pic">
      <img :src="item.product.image" width="120" height="120" srcset="img/phone-square-3@2x.jpg 2x" :alt="item.product.title">
    </div>
    <h3 class="product__title">
      {{ item.product.title }}
    </h3>
    <span class="product__code">
                Артикул: {{ item.product.id }}
              </span>

    <div class="product__counter form__counter">
      <button type="button" aria-label="Убрать один товар" @click.prevent="decrementAmount">
        <svg width="10" height="10" fill="currentColor">
          <use xlink:href="#icon-minus"></use>
        </svg>
      </button>
      <input type="text" v-model.number="amount" name="count">
      <button type="button" aria-label="Добавить один товар" @click.prevent="incrementAmount">
        <svg width="10" height="10" fill="currentColor">
          <use xlink:href="#icon-plus"></use>
        </svg>
      </button>
    </div>

    <b class="product__price">
      {{ (item.amount * item.product.price) | numberFormat }} ₽
    </b>

    <button class="product__del button-del" type="button" aria-label="Удалить товар из корзины" @click.prevent="deleteP">
      <svg width="20" height="20" fill="currentColor">
        <use xlink:href="#icon-close"></use>
      </svg>
    </button>
  </li>
</template>
<script>
  import numberFormat from "@/helpers/numberFormat";
  import {mapMutations} from "vuex";

  export default {
    filters: {numberFormat},
    props: ['item'],
    computed: {
      amount: {
        get(){
          return this.item.amount;
        },
        set(value) {
          this.updateAmount(value);
        }
      }
    },
    methods: {
      incrementAmount() {
        this.updateAmount(this.item.amount + 1);
      },
      decrementAmount() {
        this.updateAmount(this.item.amount >= 2 ? this.item.amount - 1 : 1);
      },
      updateAmount(value) {
        this.$store.dispatch('updateCartProductAmount', { productId: this.item.productId, amount: value });
      },
      deleteP() {
        this.deleteProduct()
      },
      deleteProduct(){
        this.$store.dispatch('deleteProductFromCart', {productId: this.item.productId});
      }
    }
  }
</script>