
<template>

        <header>
            <img src="/img/header.png" alt="header image">
            <h1>Välkommen till TragicBirgirs</h1>
        </header>
        <main>
            <section id="burgerSection">
            <h2> Välj birgir </h2>
                <p> Välj bland de bästa BiRgIrS </p>
            
            <div class="wrapper">
              <Burger v-for="burger in burgers"
                    v-bind:burger="burger"
                    v-bind:key="burger.name"
                    v-on:orderedBurger="addToOrder($event)"/> 
            </div>
            </section>
            <section id="informationSection">
                <h2> Ge mig all din inforamtion</h2>
                    <p> Ge mig dina personuppgifter </p>

                    <form>
                        <label for="name">För- och efternamn:</label><br>
                        <input type="text" id="name" v-model="namn" required="required" placeholder="För- och efternamn"><br> <br>
                        <label for="email">Email adress:</label><br>
                        <input type="email" id="email" v-model="email" required="required" placeholder="Email adress" ><br> <br>
                        <label for="personnummer">Personnummer:</label><br>
                        <input type="text" id="personnummer" v-model="personnummer" required="required" placeholder="Personnummer"><br><br>
                        <label for="CVC">CVC kod</label><br>
                        <input type="number" id="CVC" v-model="CVC" required="required" placeholder="CVC kod"><br><br>
                        <label for="husdjur">Namnet på ditt första husdjur:</label><br>
                        <input type="text" id="CVC" v-model="husdjur" required="required" placeholder="Snälla ge mig"><br><br>

                        <label for="payment">Betalningsmetod</label><br>
                        <select id="payment" v-model="betalningsmetod">
                            <option>Njure</option>
                            <option selected>100 000kr på rött (mest populär)</option>
                            <option>Bankrån </option>
                            <option>One sacrifical lamb </option>
                            <option>Vanligt jälva bankkort</option>
                        </select>    <br><br>
                        
                        <input type="radio" id="option1" v-model="gender" value="Man" > 
                        <label for="option1">Man</label> <br>
                        <input type="radio" id="option2" v-model="gender" value="Kvinna" > 
                        <label for="option2">Kvinna</label> <br>
                        <input type="radio" id="option3" v-model="gender" value="Vill ej berätta" checked> 
                        <label for="option3">Vill ej berätta</label> <br><br>
                        
                    </form>
            </section>
            <h3>Var ska vi leverera?</h3>    
    <div class="mapWrapper">
      <div id="map" v-on:click="setLocation">
        <div id="customerDot" v-bind:style="{left: location.x + 'px' , top: location.y +'px'}">
          T
        </div>
        
      </div>
    </div>
          
   

            <button v-on:click="sendTheForm()">
                        <img src="/img/Skicka.png" alt="skicka" style="width: 50px;">
                        beställ nu!!!!!
                    </button>
        </main>
        <hr>
    <footer>
        <p> &copy; All rights to BirgirsOnline belongs to crill </p>
    </footer>


</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io("localhost:3000");

function MenuItems(name,URL,kCal,containsGluten,containsLactose,) {
    this.name = name;
    this.URL = URL;
    this.kCal = kCal;
    this.containsGluten = containsGluten;
    this.containsLactose = containsLactose;
    

};

let menuItems = [
    new MenuItems("BeerOclockBiRgIr","/img/beerBurger.jpg" ,250,true,true),
    new MenuItems("SadBiRgIr","/img/sadburger.jpeg",450,true,true),
    new MenuItems("BigBoyBiRgIr","/img/bigboy.jpeg",850,true,false)
];

import menu from '../assets/menu.json'


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      namn: "",
      email: "",
      personnummer: "",
      CVC: "",
      husdjur: "",
      betalningsmetod:"",
      gender:"",
      orderedBurgers: {SadBiRgIr:0, BeerOclockBiRgIr:0, BigBoyBiRgIr:0},
      location: { x: 0,
            y: 0
          },
      orderIndex: 0
      

    
    }
  },
  methods: {
    getOrderNumber: function () {
      if(this.orderIndex >= 100){this.orderIndex
        this.orderIndex = 1;
      } else {
        this.orderIndex++;
      }
      
      return this.orderIndex;
    },
    setLocation: function (event) {

      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location = { x: event.clientX - 20 - offset.x,
            y: event.clientY- 20 - offset.y 
          };
        },
    sendTheForm: function () { 
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x ,
                                           y: this.location.y,
                                          namn: this.namn, 
                                          email: this.email,
                                          personnummer: this.personnummer,
                                          CVC: this.CVC,
                                          husdjur: this.husdjur,
                                          betalningsmetod: this.betalningsmetod,
                                          gender: this.gender },
                                orderItems: this.orderedBurgers
                              }
                 );
                 
    },     
  addToOrder: function (event) {
  this.orderedBurgers[event.name] = event.amount;

},
  
  }
  
}
</script>

<style>
  #map {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
    
    background: url("/img/polacks.jpg");
  }

  @import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
body {
    font-size: 12pt;
    font-family: "Gill sans", sans-serif;
}
header{
    margin: 25px;
    position: relative;
    color: rgb(181, 0, 0);
}
header h1{
    position: absolute;
    top: 38%;
    left: 33%;
}
header img{
    opacity: 0.7;
    width: 100%;
}

h2 {
    margin: 10px
}
h3 {
    margin: 10px 
}
p {
    margin: 10px 
}
form {
    margin: 10px 10px 0 10px 
} 


.gluten {font-weight: bold;}
.lactose {font-weight: bold;}
#alkohol {font-weight: bold;}
#meat {font-weight: bold;}
#dontEat {font-weight: bold;
width: 300px;}

#burgerSection {
    background-color: black;
    color: white;
    border: 2px dashed white;
}
#informationSection {
    background-color: white;
    border: 2px dashed black;
}


button{
    margin: 0 25px;
}

button:hover {
   background-color: green;
   cursor: pointer;
}

section{
    margin: 25px;
}
div{
    margin: 10px;
    padding: 10px;  
}
.wrapper {
     display: grid;
     grid-gap: 10px;
     grid-template-columns: 25% 25% 25%;
}
.mapWrapper {
    overflow: scroll;
    max-height: 500px;
  
}

#map div {
   position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
    }



</style>