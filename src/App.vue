<template>
  <div id="app">
    <namegame msg="Welcome to Your Vue.js App"/>
    <h1>Can you find {{chosenOne.firstName}} {{chosenOne.lastName}}?</h1>
    <ul>
      <li v-for="person in chosenFew" :key="person.id">
        <img :src = "person.headshot.url" @click="choosePhoto(person)">
      </li>
    </ul>
    <div class="gameMessage">{{gameMessage}}</div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'NameGame',
    data () {
    return {
      people: [],
      chosenFew: [],
      chosenOne: [],
      gameMessage: []
    }
  }, 
  created() {
    this.getPeople()
  },
  components: {
    
  },
  methods: {
    getPeople: function () { 
      axios.get("https://willowtreeapps.com/api/v1.0/profiles")
      .then((response) => {this.people = response.data;}).then(this.pickFive)
    },
    pickFive:  function () {
      Array.prototype.shuffle = function() {
        var input = this;
        for (var i = input.length-1; i >=0; i--) {
         
            var randomIndex = Math.floor(Math.random()*(i+1)); 
            var itemAtIndex = input[randomIndex]; 

            input[randomIndex] = input[i]; 
            input[i] = itemAtIndex;
        }
        return input;
    }
      var temp = this.people
      temp.shuffle()
      this.chosenFew = temp.slice(0,5)
      this.pickChosenOne()

    },
    pickChosenOne: function () {
      this.chosenOne = this.chosenFew[Math.floor(Math.random() * 5)]
    },
    choosePhoto: function (person) {
      if (this.chosenOne.id === person.id) {
        this.gameMessage = "you win!"
        setTimeout(() => {
            this.gameMessage = "";
            this.pickFive()
        }, 1000);
      }else this.gameMessage ="you lose"
       setTimeout(() => {
            this.gameMessage = "";
        }, 1000);
    }
      }
  }
</script>
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
li {
  display: inline-block;
}
img {
    position: relative;
    float: left;
    padding-right: 15px;
    width:  150px;
    height: 150px;
    background-position: 50% 50%;
    background-repeat:   no-repeat;
    background-size:     cover;
}

</style>
