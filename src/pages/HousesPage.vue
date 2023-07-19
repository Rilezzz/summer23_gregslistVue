<template>
   <div class="container-fluid text-shadow">
    <div class="row m-3">
      <div v-for="house in houses" :key="house.id" class="col-md-5 m-auto mb-3 gift-card">
        <img class=" house-img rounded" :src="house.imgUrl" :alt="house.description" >
        <div class="p-3">
      <div class="mb-3">
        <h2>Price:$ {{ house.price }}</h2>
        <h2>Year Built: {{ house.year }}</h2>
        <p>{{house.description }}</p>
        <img class="creator-picture" :src="house.creator.picture" :alt="house.creator.name">
        </div>
      </div>
      </div>
    </div>
   </div> 
</template>


<script>
import { logger } from '../utils/Logger.js';
import {computed, onMounted, onUnmounted} from 'vue'
import Pop from '../utils/Pop.js';
import { housesService } from '../services/HousesService.js';
import { AppState } from '../AppState.js';


export default {
    setup(){
        async function getHouses() {
      try {
        await housesService.getHouses();
      }
      catch (error) {
        Pop.error(error);
      }
    }
    
        onMounted(() => {
            logger.log('HOUSES PAGE MOUNTED')
            getHouses();
        }),
        onUnmounted(() =>{
            logger.log('HOUSES PAGE UN-MOUNTED')
        });
        return {
            houses: computed(() => AppState.houses )
        }
}
}
</script>


<style lang="scss" scoped>
.house-img{
  width: 40%;
  object-fit: cover;
  object-position: center;
}
.creator-picture {
  height: 5vh;
  width: 5vh;
  border-radius: 50%;
}
.gift-card{
  background-color: rgba(45, 181, 14, 0.507);
    border-radius: 2em;
    border-style: solid;
    box-shadow: 1px 1px 4px black;
    border-width: .2em;
  
}

.text-shadow {
    color: rgb(177, 169, 208);
    text-shadow: 1px 1px 4px black;
}
</style>