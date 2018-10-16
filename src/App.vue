<template>
  <div id="app">
    <h1> Mushroom identification </h1>
    <p>
      Trying to identify a mushroom? Let's find it together!
    </p>
    <form>
      <HasInput label="Does it have a cap?" v-on:input="hasCap = $event"/>
      <div id="hasCap" v-if="hasCap">
        <HasInput label="Does the cap have scales?" v-model="hasScales"/>
        <div v-if="hasScales">
          What color are the scales?
          <CircleCheckbox v-for="color in Object.keys(scaleColors)" :label="color" :value.sync="scaleColors[color]"/>
        </div>

        <HasInput label="Does it have a regular stem?" v-on:input="hasStem = $event"/>
      </div>

      <HasInput v-if="hasCap === false" label="Is it shelf-like, crust-like, or hoof-shaped?" v-on:input="shelfLike = $event"/>
      
      <div>
        What color(s) is it?
        <CircleCheckbox v-for="color in Object.keys(colors)" :label="color" :value.sync="colors[color]"/>
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
    if (obj[key]) {
      return arr.indexOf(key) > -1;
    } else {
      return false;
    }
  });

  return matchingElements.length > 0;
}

function hasAnyPositive(obj) {
  for (var key in obj) {
    if (obj[key]) {
      return true;
    }
  }
  return false;
}

function checkMatchingIf(shouldCheck, arr, obj) {
  if (shouldCheck == null) {
    return true;
  }

  if (shouldCheck && hasAnyPositive(obj)) {
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
  data() {
    return {
      hasCap: null,
      colors: {
        yellow: false,
        orange: false,
        white: false,
        buff: false,
        purple: false,
        brown: false,
        red: false,
        gray: false,
        "pale tan": false,
        black: false
      },

      hasScales: null,
      scaleColors: { white: false },

      hasStem: null,

      shelfLike: null,

      seasons: { spring: false, summer: false, fall: false },

      mushrooms: [
        {
          commonName: "Fly Agaric",
          latinName: "Amanita muscaria",
          hasCap: true,
          colors: ["yellow", "orange"],
          hasScales: true,
          scaleColors: ["white"],
          hasStem: true,
          shelfLike: false,
          seasons: ["summer", "fall"]
        },
        {
          commonName: "Destroying Angel",
          latinName: "Amanita virosa, A. verna, A. bisporigera",
          hasCap: true,
          colors: ["white"],
          hasScales: false,
          scaleColors: [],
          hasStem: true,
          shelfLike: false,
          seasons: ["summer", "fall"]
        },
        {
          commonName: "The Omnipresent Laccaria",
          latinName: "Laccaria bicolor",
          hasCap: true,
          colors: ["yellow", "buff", "orange", "purple"],
          hasScales: false,
          scaleColors: [],
          hasStem: true,
          shelfLike: false,
          seasons: ["summer", "fall"]
        },
        {
          commonName: "Aspen Bolete",
          latinName: "Leccinum aurantiacum, L. insigne",
          hasCap: true,
          colors: ["red", "brown"],
          hasScales: false,
          scaleColors: [],
          hasStem: true,
          shelfLike: false,
          seasons: ["summer", "fall"]
        },
        {
          commonName: "Birch Bolete",
          latinName: "Leccinum scabrum",
          hasCap: true,
          colors: ["yellow", "brown", "gray"],
          hasScales: false,
          scaleColors: [],
          hasStem: true,
          shelfLike: false,
          seasons: ["summer", "fall"]
        },
        {
          commonName: "Oyster Mushroom",
          latinName: "Pleurotus populinus (P. ostreatus)",
          hasCap: true,
          colors: ["white", "pale tan"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: false,
          seasons: ["summer", "fall"]
        },
        {
          commonName: "Artist's Conk",
          latinName: "Ganoderma applanatum (Fomes applanatus)",
          hasCap: false,
          colors: ["white", "gray", "brown"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: true,
          seasons: ["summer", "fall", "spring"]
        },
        {
          commonName: "False Tinder Conk",
          latinName: "Pehllinnus tremulae (Fomes ignarius)",
          hasCap: false,
          colors: ["black", "gray"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: true,
          seasons: ["summer", "fall", "spring"]
        },
        {
          commonName: "True Tinder Conk",
          latinName: "Fomes fomentarius",
          hasCap: false,
          colors: ["gray"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: true,
          seasons: ["summer", "fall", "spring"]
        },
        {
          commonName: "Birch Polypore",
          latinName: "Piptoporus betulinus (Polyporus betulinus)",
          hasCap: false,
          colors: ["white", "brown"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: true,
          seasons: ["summer", "fall", "spring"]
        },
        {
          commonName: "Multicolor Gill Polypore",
          latinName: "Lenzites betulina",
          hasCap: false,
          colors: ["brown", "gray", "yellow"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: true,
          seasons: ["summer", "fall"]
        },
        {
          commonName: "Morel (Sponge Mushroom)",
          latinName: "Morchella esculenta",
          hasCap: true,
          colors: ["gray", "yellow"],
          hasScales: false,
          scaleColors: [],
          hasStem: true,
          shelfLike: false,
          seasons: ["spring"]
        },
        {
          commonName: "Honey Mushroom",
          latinName: "Morchella esculenta",
          hasCap: true,
          colors: ["tan", "yellow"],
          hasScales: false,
          scaleColors: [],
          hasStem: true,
          shelfLike: false,
          seasons: ["fall"]
        },
        {
          commonName: "Giant Puffball",
          latinName: "Calvatia gigantea",
          hasCap: false,
          colors: ["white", "yellow"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: false,
          seasons: ["summer", "fall"]
        },
        {
          commonName: "Bear's Head Tooth",
          latinName: "Hericium coralloides",
          hasCap: false,
          colors: ["white", "yellow", "brown"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: false,
          seasons: ["summer", "fall"]
        },
        {
          commonName: "Scaly Philiota",
          latinName: "Hericium coralloides",
          hasCap: true,
          colors: ["tan", "yellow", "brown"],
          hasScales: true,
          scaleColors: ["brown"],
          hasStem: true,
          shelfLike: false,
          seasons: ["summer", "fall"]
        },
        {
          commonName: "Milk-white Toothed Polypore",
          latinName: "Irpex lacteus (Polyporus tulipiferae)",
          hasCap: false,
          colors: ["white"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: true,
          seasons: ["summer", "fall", "spring"]
        },
        {
          commonName: "Violet Polypore",
          latinName:
            "Trichaptum biforme (Hirschioporus pargamenus, Polyporus pargamenus)",
          hasCap: false,
          colors: ["brown", "purple"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: true,
          seasons: ["summer", "fall", "spring"]
        },
        {
          commonName: "Smoky Polypore",
          latinName: "Bjerkandera adusta (Polyporus adustus)",
          hasCap: false,
          colors: ["white", "gray"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: true,
          seasons: ["summer", "fall", "spring"]
        },
        {
          commonName: "Common Split Gill",
          latinName: "Schizophyllum commune",
          hasCap: false,
          colors: ["white", "gray"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: true,
          seasons: ["summer", "fall", "spring"]
        },
        {
          commonName: "Hen of the Woods/Maitake",
          latinName: "Grifola frondosa (Polyporus frondosus)",
          hasCap: false,
          colors: ["white", "gray"],
          hasScales: false,
          scaleColors: [],
          hasStem: false,
          shelfLike: true,
          seasons: ["summer", "fall"]
        }
      ]
    };
  },
  computed: {
    matchingMushrooms() {
      return this.mushrooms /*.map(m => {console.log("1) " + m.commonName); return m})*/
        .filter(m => this.hasCap == null || m.hasCap === this.hasCap)
        .filter(m => this.hasStem == null || m.hasStem === this.hasStem)
        .filter(m => checkMatchingIf(m.hasCap, m.colors, this.colors))
        .filter(m => this.hasScales == null || m.hasScales === this.hasScales)
        .filter(m =>
          checkMatchingIf(m.hasScales, m.scaleColors, this.scaleColors)
        )
        .filter(m => this.shelfLike == null || m.shelfLike === this.shelfLike)
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
