<template>
    <main class="content container">
        <div class="content__top content__top--catalog">
          <h1 class="content__title">
            Каталог
          </h1>
          <span class="content__info">
            152 товара
          </span>
        </div>

        <div class="content__catalog">
    
      <ProductFilter :price-from.sync="filterPriceFrom" :price-to.sync="filterPriceTo" :category-id.sync="filterCategoryID" :color-id.sync="filterColorID"/>

      <section class="catalog">

        <div v-if="productLoading" >
          <img src="/img/1496.gif" style="margin: 0 auto; display: block;" width="64" alt="loading">
        </div>
          <div v-if="productsLoadingFailed">Произошла ошибка...
        <button @click.prevent="loadProducts">
          Попробовать еще раз!
        </button></div>

        <ul v-if="!productLoading" class="catalog__list">
                <ProductCardPreView v-for="(product) in products" :key="product.id" :product="product"/>
        </ul>     

              <BasePagination v-model="page" :count="countProduct" :per-page="productsPerPage" />

      </section>
        </div>
      </main>
</template>

<script>
import ProductCardPreView from '@/components/ProductCardPreView.vue';
import BasePagination from '@/components/BasePagination.vue';
import ProductFilter from '@/components/ProductFilter.vue';
import axios from "axios";
import {API_BASE_URL} from "@/config";

export default {
  components: {
    ProductCardPreView,
    BasePagination,
    ProductFilter,
  },
  data() {
    return {
      filterPriceFrom: 0,
      filterPriceTo: 0,
      filterCategoryID: 0,
      filterColorID: 0,

      page: 1,
      productsPerPage: 9,

      productsData: null,

      productLoading: false,
      productsLoadingFailed: false,
    }
  },
  computed: {
    products() {
      return this.productsData
          ? this.productsData.items.map(product => {
            return {
              ...product,
              image: product.image.file.url
            }
          })
          : [];
    },
    countProduct() {
      return this.productsData
          ? this.productsData.pagination.total
          : 0;
    },
  },
  methods: {
    loadProducts() {
      this.productLoading = true;
      this.productsLoadingFailed = false;
      clearTimeout(this.loadProductsTimer);
      this.loadProductsTimer = setTimeout(() => {
        axios.get(API_BASE_URL+`api/products`, {
          params: {
            page: this.page,
            limit: this.productsPerPage,
            categoryId: this.filterCategoryID,
            minPrice: this.filterPriceFrom,
            maxPrice: this.filterPriceTo,
            colorId: this.filterColorID,
          }
        })
            .then(response => this.productsData = response.data)
            .catch(() => this.productsLoadingFailed = true)
            .then(() => this.productLoading = false);
      }, 1500);
    }
  },
  watch: {
    page() {
      this.loadProducts();
    },
    filterPriceFrom() {
      this.loadProducts();
    },
    filterPriceTo() {
      this.loadProducts();
    },
    filterCategoryID() {
      this.loadProducts();
    },
    filterColorID(){
      this.loadProducts();
    }
  },
  created() {
    this.loadProducts();
  }
}
</script>