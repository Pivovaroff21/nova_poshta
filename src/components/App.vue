<template>
  <div>
    <input type="text" @input="showCities" v-model="city" placeholder="Місто">
    <select v-if="cities" v-model="cityRef" @change="">
      <option v-for="item in cities" :value="item.Ref">{{ item.Present }}</option>
    </select>
    <p>Введіть більше 4-х символів</p>
    <hr>
    <input type="text" @input="showWarehouses" v-model="warehouseId" placeholder="Номер відділення">
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
      warehouses:[],
      warehouseRef:'',
      warehouseId:'',
    };
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
      }
    },

    showWarehouses(){
      myApi.getRequest("Address","getWarehouses",{
      SettlementRef : this.cityRef,
      WarehouseId:this.warehouseId
      })
      .then((res)=>{
        this.warehouses = res.data.data
      })
      .catch(e=>console.error(e));
    }
  }
}

</script>

<style scoped>

</style>