<template>
    <header class="welcome">
        <h1>Welcome to Burgarköket</h1>
    </header>
    <main>
        <h1>The best burgers in town</h1>
        <section id="section1">
            <h1>Select a Burger</h1>
            <p>This is where you select a burger</p>
            <div class="burgers">
                <!-- render the burgers array dynamically using the OneBurger component -->
                <Burger v-for="burger in burgers" :key="burger.name" :burger="burger" />
            </div>
        </section>
        <section id="section2">
            <h2>Costumer Information</h2>
            <h3>Delivery informaiton</h3>
            <p>
                <label for="name">Name</label><br>
                <input type="text" id="name" v-model="name" required="required" placeholder="Name">
                {{ name }}
            </p>
            <p>
                <label for="email">E-mail</label><br>
                <input type="text" id="email" v-model="email" required="required" placeholder="E-mail">

            </p>
            <p>
                <label>
                    <input type="radio" v-model="gender" value="man" checked /> Man
                </label>
                <label>
                    <input type="radio" v-model="gender" value="female" /> Woman
                </label>
                <label>
                    <input type="radio" v-model="gender" value="dono" /> Do not wish to provide
                </label>
            </p>
            <p>
            </p>
            <p>
                <label for="payment">Payment Method</label><br />
                <!-- popular pre-selected? -->
                <select id="payment" v-model="payment">
                    <option>Swish</option>
                    <option selected="selected">Card</option>
                    <option>At the restaurant</option>
                    <option>Cash</option>
                </select>
            </p>
            <div id="map-container">
                Select delivery location:
                <div id="map" class="map" v-on:click="setLocation">
                    <div class="target" :style="{ left: location.x + 'px', top: location.y + 'px' }"></div>
                </div>
            </div>
        </section>
        <button v-on:click="addOrder()" type="submit">
        <img src="/img/botton.jpg"> 
         Send Info
        </button>
    </main>
    <hr>
    <footer>
        <p>&copy; Burgers</p>
    </footer>

</template>




<script>
// import the static menu JSON and use it as the source of truth for the menu
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import imported_burgers from '../assets/burgers.json'

const socket = io("localhost:3000");

// MenuItem constructor function — use this to create product objects
// Properties: name, kCal (number), gluten (bool), lactose (bool), url (image path)

//function MenuItem(name, kCal, gluten, lactose, url) {
// this.name = name;
//this.kCal = kCal;
//this.gluten = !!gluten;     // coerce to boolean
//this.lactose = !!lactose;   // coerce to boolean
//this.url = url;
//}

// Create an initial array of burgers for the view
// Use const because we don't plan to reassign the array reference — we may mutate the contents if needed.

//const burgers = [
// new MenuItem('Chiliburger', 350, true, false, '/img/chiliburger.jpg'),
// new MenuItem('Tastyburger', 420, false, true, '/img/tastyburger.jpg'),
//new MenuItem('Hotburger', 520, true, true, '/img/hotburger.jpg')
//];

// quick check: print the array so you can inspect it in the console
//console.log('Initial burgers array:', burgers);


export default {
    name: 'HomeView',
    components: {
        Burger
    },
    props: {
    },
    data: function () {
        return {
            // initialize component data from the module-level burgers array created above
            burgers: imported_burgers,
            name: '',
            email: '',
            gender: '',
            payment: '',
            location: {
                x: 0,
                y: 0
            }
        }
    },
    methods: {
        getOrderNumber: function () {
            return Math.floor(Math.random() * 100000);
        },
        addOrder: function () {
            console.log("Order submitted by: " + this.name);
            console.log("Email: " + this.email);
            console.log("Adress: " + this.location.x + ", " + this.location.y);
            console.log("Gender: " + this.gender);
            console.log("Payment method: " + this.payment);
            console.log("Ordered burgers:" + this.burgers);



            socket.emit("addOrder", {
                orderId: this.getOrderNumber(),
                details: {
                    x: this.location.x,
                    y: this.location.y,
                    name: this.name,
                    email: this.email,
                    gender: this.gender,
                    payment: this.payment

                },
                
                orderItems: this.burgers.filter(burger => burger.amountOrdered > 0).reduce((items, burger) => {
                    items[burger.name] = burger.amountOrdered;
                    return items;
                }, {})
            });

            // Place dot at clicked location
            //this.location.x = event.clientX - 10 - offset.x;
            //this.location.y = even
        },
        setLocation: function (event) {
            var offset = {
                x: event.currentTarget.getBoundingClientRect().left,
                y: event.currentTarget.getBoundingClientRect().top
            };

            var x = event.clientX - 10 - offset.x;
            var y = event.clientY - 10 - offset.y;

            this.location.x = x;
            this.location.y = y;
            console.log(this.location)
        }
    }
}


</script>

<style>
#map-container {
    width: 500px;
    height: 300px;
    overflow: scroll;
    border: 1px solid #aaa;
}

#map {
    width: 1200px;
    /* större yta än containern */
    height: 1000px;
    /* annars ingen scroll */
    background-image: url("/img/polacks.jpg");
    background-size: cover;
    /* eller contain */
    background-repeat: no-repeat;
    background-position: top left;
    position: relative;
    /* nödvändigt för .target */
}

@import url("reset.css");
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');

.target {
    width: 15px;
    height: 15px;
    position: absolute;
    background-color: black;

}

body {
    font-family: 'Cormorant', sans-serif;
    font-size: 14pt;
    background-color: powderblue;
}

p {
    color: white;
}

h1 {
    font-family: 'Cormorant', sans-serif;
    color: white;
    font-size: 36pt;
}

main,
header,
footer,
nav ul {
    max-width: 40rem;
    margin: 0 auto 0 auto;
}

main {
    background-color: bisque;
    text-align: center;
}

/* nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
} */

.welcome {
    background-image: url("../img/restaurant.jpg");
    background-size: cover;
    overflow: hidden;
    width: 100%;
    height: 200px;
    opacity: 0.5;
}

/*
.welcome {
    margin: 50px;
    position: absolute;
    overflow: hidden;
    height: 200px;

}
*/
header h1 {
    width: 40rem;
    color: white;
    margin: 0 auto;
    text-align: center;
}

h3 {
    margin: 40px;
    text-align: center;

}

img {
    margin: 1px;
    width: 100%;
    height: auto;
}

nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
    padding: 0;
}

nav li {
    display: block;
    background-color: grey;
    padding: 1em;
}

.Very-good {
    color: green;
}

.Master {
    color: green;
    font-weight: bold;
}

.allergies {
    color: #ff5500;
    font-weight: bold;
}

.burgers {
    display: grid;
    gap: 10px;
    /* modern `gap` for row/column spacing */
    /* responsive columns: create as many columns as fit with a minimum card width 
    repeat(auto-fit, minmax(220px, 1fr));*/
    grid-template-columns: 33% 33% 33%;
    box-sizing: border-box;
}

/* Ensure images scale to fit their grid cell and keep good aspect */
.burgers img {
    width: 100%;
    height: auto;
    /* maintain aspect ratio */
    display: block;
    /* remove inline image spacing */
    margin: 0;
    /* override global img margin so cards align cleanly */
    max-height: 260px;
    /* optional cap so giant images don't take over the layout */
    object-fit: cover;
    /* crop nicely when max-height applies */
}

#section1 {
    background-color: black;
    margin: 50px;
    padding: 20px;
    border: 2px dotted #ff9900;
}

#section2 {
    margin: 50px;
    padding: 20px;
    border: 2px dotted #fe7700;
}

botton {
    text-align: center;
    margin: 10px
}

button:hover {
    background-color: red;
    cursor: pointer;
}
</style>