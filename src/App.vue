<template>
  <div id="app">
    <h1> Mushroom identification </h1>
    <p>
      Trying to identify a mushroom? Let's find it together!
    </p>
    <form>
      <HasInput label="Does it have a cap?" v-on:input="hasCap = $event"/>
      <div id="hasCap" v-if="hasCap">
        <div>
          What color(s) is the cap?
          <CircleCheckbox v-for="color in Object.keys(capColors)" :label="color" :value.sync="capColors[color]"/>
        </div>
        <HasInput label="Does the cap have scales?" v-model="hasScales"/>
        <div v-if="hasScales">
          What color are the scales?
          <CircleCheckbox v-for="color in Object.keys(scaleColors)" :label="color" :value.sync="scaleColors[color]"/>
        </div>
      </div>

      <div>
        What season did it fruit in?
        <CircleCheckbox v-for="season in Object.keys(seasons)" :label="season" :value.sync="seasons[season]"/>
      </div>
    </form>

    <h3 v-if="matchingMushrooms.length > 0"> It could be: </h3>

    <ul>
      <li v-for="mushroom in matchingMushrooms">
        {{mushroom.commonName}}
      </li>
    </ul>

  </div>
</template>

<script>
import HasInput from "./components/HasInput.vue";
import CircleCheckbox from "./components/CircleCheckbox.vue";

function hasMatchingElements(arr, obj) {
  var matchingElements = Object.keys(obj).filter(function(key) {
    if(obj[key]) {
      return arr.indexOf(key) > -1; 
    } else {
      return false;
    }
  });

  return matchingElements.length > 0;
}

function hasAnyPositive(obj) {
  for(var key in obj) {
    if(obj[key]) {
      return true;
    }
  }
  return false;
}

function checkMatchingIf(shouldCheck, arr, obj) {
  if(shouldCheck == null) {
    return true;
  }

  if(shouldCheck && hasAnyPositive(obj)) {
    return hasMatchingElements(arr, obj);
  } else {
    return true;
  }
}

function checkMatching(arr, obj) {
  return checkMatchingIf(true, arr, obj);
}

export default {
  name: "app",
  components: {
    HasInput,
    CircleCheckbox
  },
  data(){
    return {
      hasCap: null,
      capColors: {"yellow": false, "orange": false, "white": false,"buff": false,"lilac":false,"brown":false,"red":false},

      hasScales: null,
      scaleColors: {"white": false},

      seasons: {"spring": false, "summer": false, "fall": false},

      mushrooms: [
        {
          commonName: "Fly Agaric",
          latinName: "Amanita muscaria",
          hasCap: true,
          capColors: ["yellow","orange"],
          hasScales: true,
          scaleColors: ["white"],
          seasons: ["summer", "fall"]
        },
        {
          commonName: "Destroying Angel",
          latinName: "Amanita virosa, A. verna, A. bisporigera",
          hasCap: true,
          capColors: ["white"],
          hasScales: false,
          scaleColors: [],
          seasons: ["summer", "fall"]
        },
        {
          commonName: "The Omnipresent Laccaria",
          latinName: "Laccaria bicolor",
          hasCap: true,
          capColors: ["yellow", "buff", "orange", "lilac"],
          hasScales: false,
          scaleColors: [],
          seasons: ["summer", "fall"]
        },
        {
          commonName: "Aspen Bolete",
          latinName: "Leccinum aurantiacum, L. insigne",
          hasCap: true,
          capColors: ["red", "brown"],
          hasScales: false,
          scaleColors: [],
          seasons: ["summer", "fall"]
        }
      ]
    }
  },
  computed: {
    matchingMushrooms() {
      return this.mushrooms/*.map(m => {console.log("1) " + m.commonName); return m})*/
                           .filter(m => this.hasCap == null || m.hasCap === this.hasCap)
                           .filter(m => checkMatchingIf(m.hasCap, m.capColors, this.capColors))
                           .filter(m => this.hasScales == null || m.hasScales === this.hasScales)
                           .filter(m => checkMatchingIf(m.hasScales, m.scaleColors, this.scaleColors))
                           .filter(m => checkMatching(m.seasons, this.seasons));
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

form {
  user-select: none;
}

label {
  cursor: pointer;
}

div {
  margin-top: 6px;
  margin-bottom: 6px;
}
</style>
