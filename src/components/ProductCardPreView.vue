<template>
  <li class="catalog__item">
    <router-link class="catalog__pic" :to="{name: 'product', params: {id: product.id}}">
      <img :src="product.image" :alt="product.title">
    </router-link>

    <h3 class="catalog__title">
      <a :href="product.link">{{ product.title }}</a>
    </h3>

    <span class="catalog__price">{{ product.price | numberFormat }} ₽</span>

    <ul class="colors colors--black">
    <li class="colors__item" v-for="color in product.colors" :key="color.id">
      <label class="colors__label">
        <input class="colors__radio sr-only" type="radio" :name="product.title" :value="color.code" :checked="color.id === selectedColorId">
        <span class="colors__value" :style="'background-color: ' + color.code"></span>
      </label>
    </li>
    </ul>
  </li>
</template>

<script>
  import gotoPage from "@/helpers/gotoPage";
  import numberFormat from '@/helpers/numberFormat';

  export default {
    methods: {
      gotoPage
    },
    filters: {
      numberFormat
    },
    props: {
      product: Object,
    }
  }
</script>