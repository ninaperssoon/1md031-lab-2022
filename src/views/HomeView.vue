<template>
    <body>
    <header>
        <img src="https://img.freepik.com/vector-premium/burger-seamless-pattern-background-vector-design_22159-34.jpg?w=2000" id="headerimage">
        <h1 id="headline"> Welcome to Nina's Burger Heaven</h1>
    </header>
    <main>
        <section id="burger">
            <h2> Select burgers</h2>
            <div> This is where you execute burger selection</div>
              <div>
              <div class="wrapper">

    <Burger v-for="burger in burgers" 
            v-bind:burger="burger" 
            v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)" />
    </div>

  </div>
        </section>
        <section id="contact">
            <h2> Customer Information</h2>
            <div> This is where you provide necessary information</div>
            <div>
                <h3>Delivery information:</h3>
                <form>
                <p>
                    <label for="fullname">Full name</label><br>
                    <input type="text" id="fullname" v-model="fn" required="required" placeholder="First- and Last name">
                </p>
                <p>
                    <label for="email">E-mail</label><br>
                    <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
                </p>
               
            <p>
                <label for="payment">Payment options</label><br>
                <select id="payment" v-model="po">
                    <option>Credit card</option>
                    <option>Klarna</option>
                    <option>Swish</option>
                    <option>Cash</option>
                </select>
             </p>
             
                <p>Gender:</p>
               <input type="radio" id="male" v-model="gender" value="male">
               <label for="male">Male</label><br>
               <input type="radio" id="female" v-model="gender" value="female">
               <label for="female">Female</label><br>
               <input type="radio" id="non-binary" v-model="gender" value="non-binary">
               <label for="non-binary">Non-binary</label><br>
               <input type="radio" id="undisclosed" v-model="gender" value="undisclosed">
               <label for="undisclosed">Undisclosed</label>

             </form>  
            </div>
            <h4>Please indicate point of delivery:</h4>
            <div id="overFlow">
             
      <div id="map" v-on:click="setLocation">
        <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
        <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}" v-bind:key="'dots' + key">
            T
          </div>
      </div>
      </div>
    </div>
        </section>
        <button type="submit" class="pointer" v-on:click="printOrder(key)">
            <img src="https://images.vexels.com/media/users/3/230881/isolated/preview/00aab0971c04fefcb63ba57cd62522fc-happy-hamburger-gradient.png"style="height: 30px">
            Place my order!
          </button>
    </main>
    <hr>
    
    <footer>
      &copy; 2022 Nina Inc.
    </footer>

   </body>
   

</template>


<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

// function MenuItem(name, url, kCal, gluten, lactose){
//   this.name = name;
//   this.url = url;
//   this.kCal = kCal;
//   this.gluten = gluten;
//   this.lactose = lactose;
// }

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  created: function () {
    socket.on('deliveryTimeIs', time =>
    this.deliveryTime = time);  
  },
  
  data: function () {
    return {
      orderedBurgers : {},
      burgers: menu,
      fullName : null,
      email : null,
      gender : null,
      payment : null,
      fn : null,
      em : null,
      po : null,
      hn : null,
      sm : null,
      location: { x: 0,
                  y: 0
                },
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
      
    },
    
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      console.log(JSON.stringify(this.orderedBurgers));
      
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location = {
        x: event.offsetX-30,
        y: event.offsetY-30
      }
    
    },
    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location = {
        x: event.offsetX-25,
        y: event.offsetY-25
      }
    },
    printOrder: function(key) {
      var fullName = this.fn;
      var email = this.em;
      var gender = this.gender;
      var payment = this.po;
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y,
                                           fullName: this.fn,
                                           email: this.em,
                                           payment: this.po,
                                           gender: this.gender },
                                orderItems: this.orderedBurgers
                                
                              }
                 );
    }
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Rubik:wght@300&display=swap');


body {
    font-family: "Rubik", sans-serif;
    font-size: 1.3em;
 }
 section{
    margin: 2em;
    padding: 1em;
    border: 0.3em dashed;
 }
 div {
    padding: 0em;
    margin: 0.8em;
 }
 header {
    margin-left: 2em;
    margin-right: 2em;
    height: 10em;
    overflow: hidden;
 }
 #headline {
    margin-top: -36.5em;
    padding: 1.4em;
    position: absolute;
 }
 #headerimage {
    opacity: 0.5;
 }


.allergen {
    font-weight: bold;
}
#burger {
    background-color: black;
    color: white;
}
button:hover {
    background-color: lightblue;
 }
.pointer {
    cursor: pointer;
    margin: 1em 5em;
  }
  .wrapper {
    display: grid;
    grid-gap: 1em;
    grid-template-columns: 15em 15em 15em;
    background-color: black;
    color: white;
}
.box {
    background-color: black;
    color: #fff;
}
#map {
  width: 1920px;
  height: 1078px;
  background: url('../../public/img/polacks.jpg');
  cursor: pointer;
  
}
#overFlow {
  width: 1200px;
  height: 500px;
  overflow : scroll;
}
#map:hover {
  background-color: pink;
}
#dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
    font-size: 17px;
  }
#dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
</style>