<template>
        <div>   
            <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
              
  <a class="navbar-brand" href="#">
    <img src="../assets/logo.png" width="30" height="30" class="d-inline-block align-top" alt="">
    Vue.gg
  </a>

  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse d-flex justify-content-end px-5" id="navbarSupportedContent">
                    <div class="col-sm-3">
                    <select @change="srv" class="form-control">
                        <option value="tr1">TURKEY</option>
                        <option value="na1">NORTH AMERICA</option>
                        <option value="euw1">EU WEST</option>
                        <option value="eun1">EU NORDIC EAST</option>
                        <option value="la1">LATIN AMERICA NORTH</option>
                        <option value="la2">LATIN AMERICA SOUTH</option>
                        <option value="br1">BRAZIL</option>
                        <option value="ru1">RUSSSIA</option>
                        <option value="oc1">OCENIA</option>
                        <option value="jp1">JAPAN</option>
                        <option value="kr">KOREA</option>
                    </select>
                    </div>
               
              
 
  
  

        
            <form class="form-inline my-2 my-lg-0 px-2 ">
            <input class="form-control mr-sm-2" type="search" placeholder="Sihirdar Adı" aria-label="Search" v-model="name">
            <button @click="verial" class="btn btn-outline-success my-2 my-sm-0" type="submit">Ara</button>
            </form>
        </div>
        </nav>
        </div>
    
</template>""
<script>
import axios from 'axios'

        export default {
            data () {
                return{
                name : "",
                server : "tr1",
                
                }
            },
          
            methods : {
                verial(){
                     var api ="RGAPI-89a91767-72fc-4abd-ae26-7b1a1620c28d";
                    axios.get("https://"+this.server+".api.riotgames.com/lol/summoner/v4/summoners/by-name/"+this.name+"?api_key="+api)
                    .then(response => {
                         
                          let informationpack = {
                              apikey : api,
                              name : response["data"]["name"],
                              level: response["data"]["summonerLevel"],
                              ico : "http://opgg-static.akamaized.net/images/profile_icons/profileIcon"+response["data"]["profileIconId"]+".jpg" ,
                              id : response.data.id,
                              accountid : response.data.accountId,
                              server :this.server

                          }
                        this.$emit("informationevent", informationpack)
                        console.log(informationpack.accountid)

                    })
                     
                },
                srv(){
                    this.server = event.target.value
                    
                }
            }
        }
</script>
<style>

</style>

