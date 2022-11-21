<template>
 <div>   
                <h3>{{ burger.name }}</h3>
                <img v-bind:src="burger.url" alt="Span" title="burger.name" style="height: 200px">
                <ul>
                    <li> {{ burger.kCal }} kCal </li>
                    <li v-if="burger.gluten"> Contains <span class="allergen">gluten</span></li>
                    <li v-if="burger.lactose"> Contains <span class="allergen">lactose</span></li>
                    <!-- <button v-on:click="selectThisBurger">Select</button> -->
                    <button v-on:click="decreaseBurger">-</button>
                    <button v-on:click="increaseBurger">+</button>
                    {{amountOrdered}} 
                </ul>
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
        amountOrdered:0,
      }
    },
    methods: {
      // selectThisBurger: function () {
      //   this.$emit("selected", this.burger)
      // }
  
      increaseBurger: function () {
        this.amountOrdered++;
        this.$emit('orderedBurger', { name:   this.burger.name, 
                                      amount: this.amountOrdered 
                              }
        );

        
      },
      decreaseBurger: function () {
        this.amountOrdered--;
        this.$emit('orderedBurger', { name:   this.burger.name, 
                                      amount: this.amountOrdered 
                              }
        );
      }

      
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
   div {
    padding: 1em;
    margin: 0.8em;
 }
 .allergen {
    font-weight: bold;
}
  </style>
  