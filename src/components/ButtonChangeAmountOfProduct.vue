<template>
  <div class="form__counter">
    <button type="button" aria-label="Убрать один товар" @click="decrement">
      <svg width="12" height="12" fill="currentColor">
        <use xlink:href="#icon-minus"></use>
      </svg>
    </button>

    <input type="text" :value="productAmount" @input="updateProductAmount">

    <button type="button" aria-label="Добавить один товар" @click="increment">
      <svg width="12" height="12" fill="currentColor">
        <use xlink:href="#icon-plus"></use>
      </svg>
    </button>
  </div>
</template>

<script>
export default {
  props: ['productAmount'],
  methods: {
    increment() {
      this.$emit('update:productAmount', this.productAmount + 1);
    },
    decrement() {
      this.$emit('update:productAmount', this.productAmount >= 2 ? this.productAmount - 1 : 1);
    },
    updateProductAmount(event) {
      const newValue = parseInt(event.target.value, 10);
      if (!isNaN(newValue)) {
        this.$emit('update:productAmount', newValue);
      }
    },
  }
}
</script>
