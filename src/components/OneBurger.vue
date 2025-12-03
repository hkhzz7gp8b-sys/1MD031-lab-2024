<template>
  <div>
    <h3>
    {{ burger.name }} 
    </h3>
    <img v-bind:src="burger.url" v-bind:alt="burger.name" width="300px"/>
    <p v-if="burger.gluten">innehåller <span class="gluten"> gluten</span> </p>
    <p v-if="burger.lactose">innehåller <span class="lactose"> laktos</span> </p>
    {{  burger.kCal + " kCal"  }} 
      <br><br>
      {{ "how many?" }} <br>
      <button v-on:click="decreaseAmountOrderd()">-</button>
    {{amountOrdered}}
     <button v-on:click="increaceAmountOrderd()">+</button>
  </div>




</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function () {
  return {
    amountOrdered: 0,
  }
},
methods: {
  increaceAmountOrderd() {
    this.amountOrdered++;
    this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
  );
  },
  decreaseAmountOrderd() {
    if (this.amountOrdered > 0) {
      this.amountOrdered--;
    }
  }
}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>