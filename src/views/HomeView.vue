<script setup>
  import ipresults from "@/components/ipresult.vue"
  import {ref, onMounted} from "vue"
  import L from "leaflet"
  import axios from "axios";
  let isresult = ref(false)
  let map = ref(null)
  let ip = ref("")
  let ipInfo  = ref(null)

  const onClosewindow = (value)=>{
    ipInfo.value = value
  }

  onMounted(()=>{ 
        map.value = L.map('map').setView([13.006367, 77.614566], 9)
        L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
            maxZoom: 19,
            attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
        }).addTo(map.value);
    })

  const getIp = async()=>{
    if(ip.value.length === 0){
      alert("enter the ip address")
      return
    }
    isresult.value = true
    try{
      let response = await axios.get(`https://geo.ipify.org/api/v2/country,city?apiKey=at_AuUh42QuM5lWWW2O2ucV6G0Mxiarm&ipAddress=${ip.value}`)
      let result = response.data
      console.log(result)
      ipInfo.value = {
        address : result.ip,
        location : result.location.region,
        timezone : result.location.timezone,
        isp : result.isp
      }
      L.marker([result.location.lat, result.location.lng]).addTo(map.value);
        map.value.setView([result.location.lat, result.location.lng], 13)
    }
    catch(error){
      alert(error.message)
    }
    ip.value = ""
  } 
</script>

<template>
  <div class="flex flex-col h-[120vh] max-h-screen bg-slate-300">
    <div class="flex justify-center w-full h-[45vh] items-center bg-[url('https://www.shutterstock.com/image-vector/map-world-worldmap-global-silhouette-260nw-2025850361.jpg')] ">
      <div class="w-[60%]  flex flex-col items-center space-y-3" >
        <h1 class="text-4xl font-bold text-blue-100 mt-8 md:mt-2">IP Address Tracker <i class="fas fa-map-marker-alt"></i> </h1>
        <div class="flex w-full">
          <input type="search" placeholder="Enter the Ip address.." v-model="ip" class="flex-1 py-2 px-4 rounded-md focus:outline-none">
          <button @click="getIp" ><i class="fas fa-search w-12 h-12 pt-4 ml-2 text-white rounded-full bg-blue-800"></i></button>
        </div>
        <div v-if="ipInfo" class="flex mx-auto justify-center">
          <ipresults class="z-20"
          :ipInfo="ipInfo"
          @closewindow="onClosewindow"
          />
        </div>
      </div>
    </div>
    <div id="map" class="z-10 " ></div>
    <footer class="mt-4 bg-slate-300 flex items-end justify-end py-3 px-10 ">
      <div>
        <p>&copy;shaikyaseen2704@gmail.com</p>
        <p><i class="fas fa-phone text-sm mr-1"></i>+91 6362386934</p>
      </div>
    </footer>
  </div>

</template>

<style scoped>
@import url('https://unpkg.com/leaflet@1.9.4/dist/leaflet.css');
#map{
  width: 80%;
  height: 100%;
  margin:0px auto;
}
#map:focus{
  outline: none;
}
@media (max-width:800px) {
  #map{
    width: 100%;
  }
  footer{
    margin : 0;
    padding: 10px;
  }
}
</style>