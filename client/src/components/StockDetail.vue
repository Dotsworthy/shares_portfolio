<template lang="html">
  <div v-if="stock" id="StockDetail" class="stock-detail">
      <div class="stock-detail-item">
        <h2>{{stock.name}}</h2>
        <p>You own <span>{{stock.shares}}</span> shares worth <span>$ {{stock.totalValue}}</span></p>
      </div>
      <div class="stock-detail-form">
        <form @submit.prevent="handleUpdate">
          <label for="shares-number">Shares: </label>
          <input type="number" name="shares-number" v-model="updatedStock.shares" required>
          <input type="submit" value="Update Shares">
        </form>
      </div>
  </div>
</template>

<script>

import { eventBus } from '@/main.js'

export default {
  data() {
    return {
      updatedStock: {
        _id: this.stock._id,
        shares: null,
        latestCloseValue: this.stock.closeValues[99][1]
      }
    }
  },
  name: 'stock-detail',
  props: ['stock'],
  methods: {
    handleUpdate() {
      this.stock.shares = this.updatedStock.shares
      this.stock.totalValue = parseInt(this.updatedStock.latestCloseValue * this.updatedStock.shares)
      eventBus.$emit('updated-stock', this.updatedStock)
      this.updatedStock.shares = null
    }
  },
  computed: {

  }
}
</script>

<style lang="css" scoped>

  .stock-detail {
    width: 40%;
    display: flex;
    margin: 0 30%;
    flex-wrap: wrap;
    background-color: #4c4c4d;
    border-radius: 2em;
  }

  .stock-detail-item, .stock-detail-form, .stock-detail-buy {
    width: 100%;
    text-align: center;
  }

  .stock-detail-item {
    font-size: 1.5em;
  }

  .stock-detail-item h2 {
    padding: 0.5em 0;
  }

  .stock-detail-item p {
    font-size: 1.2em;
  }

  .stock-detail-item p span{
    color: #F4B41A;
  }

  .stock-detail-form {
    margin: 0.5em 0;
  }

  .stock-detail-form label {
    font-size: 1.5em;
  }

  .stock-detail-form input[type="number"]{
    padding: 0.5em 1em;
    border-radius: 2em;
    outline: none;
    border: none;
    text-align: center;
    font-size: 1em;
  }

  .stock-detail-form input[type="submit"] {
    background-color: #F4B41A;
    border: 1px solid ghostwhite;
    border-radius: 2em;
    color: ghostwhite;
    padding: 0.5em 1em;
    font-size: 1em;
    margin: 1em;
  }

  .stock-detail-buy input:hover {
    background-color: #f6be3c;
    cursor: pointer;
  }

</style>
