<template>
  <div class="h-screen relative overflow-hidden">

 
<img :src="background" />
<div class="absolute w-full h-full top-0 overlay" />
<div class="absolute w-full h-full top-0 p-48"> 
<div class="flex justify-between">
  <div>
  <h1 class="text-7xl text-white"> {{city.name}}</h1>
  <p class="font-extralight text-2xl mt-2 text-white">Sunday Dec 14th</p>
  <img  
  :src="`https://openweathermap.org/img/wn/${city.weather[0].icon}@4x.png`" class="w-56 icon"/>
</div>
<div>
  <p class="text-9xl text-white font-extralight"> {{city.main.temp}}°</p>
</div>
</div>
<div class="mt-20">
<input type="text" class="w-1/2 h-10" 
placeholder="Find a city"
 v-model="input"/>
<button class="bg-sky-400 w-20 ml-2 text-white h-10" @click="handleClick">Search</button>
</div>
</div>

  </div>
</template>
<script setup>
const search =ref("Toronto");
const input = ref("");
const background = ref("");


const handleClick = () =>{
  const formatedSearch = input.value.trim().split(" ").join("+");
  search.value = formatedSearch;
  input.value ="";
  refresh() ;
  // the refresh is one way of getting a state if the values have changed and we pass it as a 3rd parameter for values defined in the useAsyncData and this state is updated when the handleClick method is excecuted and the input value is null.This is one way of getting state which has changed. The second one will be to use the watch inside the useAsyncData method, by passing the watch as a 3rd parameter.
}
   // const {data:city,error} = useFetch(
    // ()=> `https://api.openweathermap.org/data/2.5/weather?q=${search.value}&units=metric&appid=80a523dd90940a52b51ddf92ad9d0185`)
    // Not the use of useFetch composables is limited because it will only return the requested url but if we want to run other logic when the url returns a value we make use of the useAsyncData composables as below


    const {data:city, error, refresh} = useAsyncData("city", async () => {
      const response = await $fetch(`https://api.openweathermap.org/data/2.5/weather?q=${search.value}&units=metric&appid=80a523dd90940a52b51ddf92ad9d0185`);

      const temp = response.main.temp;
      if(temp <= -10){
        background.value = "https://images.unsplash.com/photo-1483664852095-d6cc6870702d?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3540&q=80";
      }
      else if (temp > -10 && temp <= 0){
        background.value ="https://images.unsplash.com/photo-1476820865390-c52aeebb9891?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3540&q=80";
      }
      else if( temp > 0 && temp <= 10){
        background.value ="https://images.unsplash.com/photo-1560258018-c7db7645254e?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=4032&q=80";
      }
      else{
        background.value = "https://images.unsplash.com/photo-1507525428034-b723cf961d3e?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3546&q=80";
      }
      return response;
     
    }, {
      // this is how to watch values that might change
      watch:[search]
    })

   


</script>
<style scoped>
.overlay{
  background-color:rgba(0, 0, 0, 0.5) ;
}
.icon{
  margin-left:-2.75rem;
  margin-top:-2.75rem; ;
}
</style>