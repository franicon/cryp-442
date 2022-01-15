<template>
  <div class="container">
     <div class="banner">
        <div class="mt-5">
          <h1 class="fs-1 text-light text-sub-header">Track <span class="header-text">Crypto</span></h1>
          <h1 class="fs-1 text-light header-text-light "> Prices by Market Cap</h1>
          <p class="small-text">coin42 allows you to monitor diversed cryptocurrency market, trade without limits</p>
        </div>
     </div>
  
    <div class="row g-3">
      <div class="col-12">
        <input type="text" class="form-control" placeholder="Last name" aria-label="Last name" v-model="search">
        
      </div>
    </div>

    
      <div class="card-wrapper my-5">
      <div class="" v-if="error">opps something wen't wrong check your connection</div>
        <div class="row" v-if="coins.length">
          <div class="col-lg-3 col-md-4 col-12 col-sm-6" v-for="coin in match" :key="coin">
            <div class="card my-2 p-3">
              <div class=" d-flex justify-content-between">
                <div class="me-auto my-2"><img :src="coin.image" width="40" alt=""></div>
                  <div class="my-2">
                    <span class="badge rounded-pill bg-danger" v-if="coin.price_change_percentage_24h < 0 ">{{coin.price_change_percentage_24h.toFixed(2)}} %</span>
                  <span class="badge rounded-pill bg-success" v-if="coin.price_change_percentage_24h > 0 ">{{coin.price_change_percentage_24h.toFixed(2)}} %</span>
                  </div>
              </div>
                <p class="name">{{coin.name}}</p>
              <h4><b>${{coin.current_price.toLocaleString()}}</b></h4>
            </div>
          </div>
      </div>
      <div class="d-flex justify-content-center text-light flex-column" v-else>
        <div class="spinner-border" role="status">
        </div>
        <div class="">Plase Wait</div>
      </div>
     
      </div>
  </div>
</template>

<script>

import {onMounted,ref,computed} from 'vue'
import axios from 'axios'
export default {
  name: 'Home',
  setup() {
    let coins = ref([])
    let search = ref('')
    onMounted(() => {
      axios.get('https://api.coingecko.com/api/v3/coins/markets?vs_currency=ngn&order=market_cap_desc&per_page=100&page=1&sparkline=false')
      .then((res)=>{
        console.log(res.data)
        coins.value = res.data
        
      })
      .catch((error)=>{
        console.log(error)
      })
    })
    
    const match = computed(()=>{
        return coins.value.filter((coin)=>  coin.name.toLowerCase().includes(search.value.toLowerCase()))
       
    })
    return{
      coins,
      search,
      match
      
    }
  }
}
</script>
