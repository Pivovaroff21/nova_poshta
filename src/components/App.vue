<template>
  <div>
    <input type="text" @input="showCities" v-model="city">
    <p>Введіть більше 4-х символів</p>
    <select v-if="cities" v-model="cityRef" @change="showWarehouses">
      <option v-for="item in cities" :value="item.Ref">{{ item.Present }}</option>
    </select>
    <hr>
    <select v-if="warehouses" v-model="warehouseRef">
      <option v-for="item in warehouses" :value="item.SiteKey">{{ item.Description }}</option>
    </select>
  </div>
</template>

<script>
import { myApi } from '../api';
export default {
  name:"App",
  data() {
    return {
      currentArea:"",
      cities:[],
      city:'',
      cityRef:'',
      warehouses:'',
      warehousesRef:''
    };
  },
  mounted(){

    // myApi.getRequest("Address","searchSettlements",{CityName : "за"})
    // .then((res)=>{
    //   console.log(res.data)
    //   this.areas = res.data.data.Addresses
    // })
    // .catch(e=>console.error(e));



   },
  methods:{
    showCities(){
      if(this.city.length>3){
        myApi.getRequest("Address","searchSettlements",{
        CityName : this.city,
        Limit : "10",
        Page : "1"
      })
    .then((res)=>{
      const dataObj = res.data["data"]
      this.cities = dataObj[0].Addresses
      console.log(dataObj)
    })
    .catch(e=>console.error(e));
      }
  },

  showWarehouses(){
        myApi.getRequest("Address","getWarehouses",{
        SettlementRef : this.cityRef,
      })
    .then((res)=>{
      this.warehouses = res.data.data
      console.log(res.data)
    })
    .catch(e=>console.error(e));
      }
  }
}

</script>

<style scoped>

</style>