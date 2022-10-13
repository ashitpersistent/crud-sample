<template>
  <div class="home">
    <form onsubmit="return false" >
      <button @click="generateCard">Add</button>
      <input type="text"   v-model="inp">
    </form>

    <div class="cards">
      <Card
      v-for="loc in weatherLocation"
      :key="loc.id"
      >
        <template v-slot:delete>
          <button @click="deleteEl(loc.id)">delete</button>
        </template>
        <template v-slot:update>
            <button @click="updateEl(loc.id)">edit</button>
            <input type="text"
            v-model="updateVal"

            v-if="flags.checkUpdateStatus"
            >
            <button
            v-if="flags.checkUpdateStatus"
            @click="saved(loc.id)"
          >update</button>
        </template>
        <template v-slot:title>
          {{loc.location}}
        </template>

        <template v-slot:content >
          <img src="https://www.google.com/maps/vt/data=JDzAvDVk4ptHwCeUBZbtFsTvzO9B9YMnvFQJvN1ywS3mD3kqWY7woH8wQKAgj0p56pqBCz7AmiNv5lI6WmOYYccp60MXPTvKp7nS3Eh3avliWd62_-dtOmHYh5t_QY3PeX2xoWJYDN-NPdddYjYeIjLL5heMfR5DplFDYzPOeDFYZr_m_w_O7PtBADg2h4438mslL-yfbn1RHhJwqUsMVjAZO8CDw9YPnkb-NmdrAai5aQD7aRirqHt0eDwfrXF3qVw" alt="">
        </template>

         <template v-slot:description >
          {{"sample"}}
        </template>

      </Card>
    </div>
  </div>
</template>

<script lang="ts">
import Card from '@/components/Card.vue'
import { reactive,ref} from 'vue'
export default{
  components:{Card},



  setup(){

    let weatherLocation = reactive([{id:0,location:""}]);
    let inp = ref("")
    let updateVal = ref("")
    let id= ref(0)
    weatherLocation.pop()

    const flags = reactive({
      checkUpdateStatus:false
    })
    
    const generateCard = () =>{
      let obj=reactive({
      id:id.value,
      location:inp.value,
      });
      if(inp.value){
        weatherLocation.push(obj)
      }
      id.value+=1;
    };


    const deleteEl = (a:number) =>{
            // weatherLocation.splice(a,1)
              let i=0;
              for(i=0;i<weatherLocation.length;i++){
                if(a==weatherLocation[i].id){
                  break;
                }
              }
      
            weatherLocation.splice(i,1)
            console.log(a,"found",weatherLocation[a].id)
            console.log(weatherLocation)
    }

    const updateEl = (a:number) =>{
      console.log("updating..",a)
      flags.checkUpdateStatus = true;

    }    

    const saved = (a:number)=>{
      flags.checkUpdateStatus = false;
      console.log("saved")
      let i=0;
              for(i=0;i<weatherLocation.length;i++){
                if(a==weatherLocation[i].id){
                  break;
                }
      }
      weatherLocation[i].location = updateVal.value

    }
    return{
      generateCard,
      weatherLocation,
      inp,
      deleteEl,
      updateEl,
      flags,
      saved,
      updateVal
    }
  }
}
</script>

<style scoped>

.opace{
  opacity: 0.5;
}
.cards{
  display:flex
}
.card:hover{
  opacity: 1.0;
}

img{
  width: 100%;
}




</style>
