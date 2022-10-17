<template>
  <div class="home">
    <form onsubmit="return false">
      <button @click="generateCard">Add</button>
      <input type="text" v-model="inp">
    </form>

    <div class="cards">
      <Card v-for="loc in weatherLocation" :key="loc.id">
        <template v-slot:delete>
          <button @click="deleteEl(loc.id)">delete</button>
        </template>
        <template v-slot:update>
          <button @click="updateEl(loc.id)">edit</button>
          <input type="text" v-model="updateVal" v-if="flags.checkUpdateStatus">
          <button v-if="flags.checkUpdateStatus" @click="saved(loc.id)">update</button>
        </template>
        <template v-slot:title>
          {{loc.location}}
        </template>

        <template v-slot:content>
          <img :src=locJsonVal alt="">
        </template>

        <template v-slot:description>
          {{"description"}}
        </template>

      </Card>
    </div>
  </div>
</template>

<script lang="ts">
import Card from '@/components/Card.vue'
import { reactive, ref } from 'vue'

export default {
  components: { Card },



  setup() {

    let weatherLocation = reactive([{ id: 0, location: "" }]);
    let inp = ref("")
    let updateVal = ref("")
    let id = ref(0)
    let locJsonVal = ref(null) as any
    weatherLocation.pop()

    const flags = reactive({
      checkUpdateStatus: false
    })

    async function getApi() {
      const info = await fetch("http://en.wikipedia.org/w/api.php?action=query&prop=pageimages&format=json&origin=*&piprop=original&titles=" + inp.value)
      const json = await info.json();
      const locJson = json
      let temp = locJson.query.pages
      temp = temp[Object.keys(temp)[0]]
      locJsonVal.value = temp.original.source
      console.log(locJsonVal.value)
    }

    const generateCard = () => {
      getApi()
      let obj = reactive({
        id: id.value,
        location: inp.value
      });
      if (inp.value) {
        weatherLocation.push(obj)
      }
      id.value += 1;
    };


    const deleteEl = (a: number) => {
      // weatherLocation.splice(a,1)
      let i = 0;
      for (i = 0; i < weatherLocation.length; i++) {
        if (a == weatherLocation[i].id) {
          break;
        }
      }

      weatherLocation.splice(i, 1)
      console.log(a, "found", weatherLocation[a].id)
      console.log(weatherLocation)
    }

    const updateEl = (a: number) => {
      console.log("updating..", a)
      flags.checkUpdateStatus = true;

    }




    const saved = (a: number) => {
      flags.checkUpdateStatus = false;
      console.log("saved")
      let i = 0;
      for (i = 0; i < weatherLocation.length; i++) {
        if (a == weatherLocation[i].id) {
          break;
        }
      }
      weatherLocation[i].location = updateVal.value

    }
    return {
      generateCard,
      weatherLocation,
      inp,
      deleteEl,
      updateEl,
      flags,
      saved,
      updateVal,
      locJsonVal
    }
  }
}
</script>

<style scoped>
.opace {
  opacity: 0.5;
}

.cards {
  display: flex
}

.card:hover {
  opacity: 1.0;
}

img {
  width: 100%;
}
</style>
