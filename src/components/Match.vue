<template>
    <div>
            <div class="card  " style="background-color:#35495e;height:350px">
                    <h6 class="card-header">{{gameType}}
                      
                    </h6>
                    <div v-for="item in info">
                        <div class="d-flex justify-content-start ">
                                 <div class="ml-3 mt-4" style="background-color:#35495e; color:white" >
                                   <img  :src="item.ico"  style= "width:100px; height:100px ;"/>
                                    <div >
                                        <p class="d-flex justify-content-center" style="font-size:10px">{{item.name}}</p>
                                        <p class="d-flex justify-content-center" style="font-size:10px">{{item.level}}</p>
                                    
                                    </div>
                                    </div>
                                   
                        </div>
                       <hr>
                        <div class="d-flex justify-content-center  ml-4  " style="color:#9cdcfe;border-bottom: 2px solid #ff1420">
                            <div class="float-left ml-3">
                                
                                        <p class="float-left ml-5" style="font-size:15px">{{players[5]}}</p>
                                        <p  class="float-left ml-5" style="font-size:15px">{{players[6]}}</p>
                                        <p class="float-left ml-5" style="font-size:15px">{{players[7]}}</p>
                                        <p class="float-left ml-5" style="font-size:15px">{{players[8]}}</p>
                                        <p class="float-left ml-5" style="font-size:15px">{{players[9]}}</p>
                                        
                            </div>
                        </div>
                      <div class="d-flex justify-content-center  ml-4 mt-1   " style="color:#ff1420;border-bottom: 2px solid #41b883">
                            <div class="float-left ml-3">
                                
                                        <p class="float-left ml-5" style="font-size:15px">{{players[0]}}</p>
                                        <p  class="float-left ml-5" style="font-size:15px">{{players[1]}}</p>
                                        <p class="float-left ml-5" style="font-size:15px">{{players[2]}}</p>
                                        <p class="float-left ml-5" style="font-size:15px">{{players[3]}}</p>
                                        <p class="float-left ml-5" style="font-size:15px">{{players[4]}}</p>
                                        
                            </div>
                        </div>
                    </div>
            </div>


    </div>
</template>

<script>
import Chart from "chart.js";
import Axios from 'axios';
export default {
  props : ["info","show"], 
  data(){
        return{
            gamesId : [],
            gamesLane : [],
            gamesChampion : [],
            gameType : "ağağağa",
            players : []

        }
  },
  mounted(){
        this.matchdatafetch()
        this.addCharts()
  },
  methods:{
        matchdatafetch(){
            let url = "https://"+this.info[0].server+".api.riotgames.com/lol/match/v4/matchlists/by-account/"+this.info[0].accountid+"?api_key="+this.info[0].apikey
            Axios.get(url).then(response =>{
                
                var datalist = []
                var i;
                for(i=0;i<5;i++){
                    datalist.push(response.data.matches[i])
                   this.gamesId.push(datalist[i].gameId) 
                  
                   this.gamesChampion.push( datalist[i].champion)
                  
                
                }
            
                this.fetchistory()
            })

        },
        fetchistory(){
             let url = "https://"+this.info[0].server+".api.riotgames.com/lol/match/v4/matches/"+this.gamesId[0]+"?api_key="+this.info[0].apikey
             Axios.get(url).then(response =>{
                console.log(response)
                console.log(url)
                 
                 if(response.data.gameMode=="CLASSIC"){
                     this.gameType="Normal Oyun"
                 }else{
                     this.gameType=="Dereceli Oyun"
                 }
                 var i;
                 for(i=0;i<10;i++){
                     this.players.push(response.data.participantIdentities[i].player.summonerName)
                     if(this.info[0].name == response.data.participantIdentities[i].player.summonerName ){
                         
                        this.gamesLane.push(response.data.participants[i].stats)

                     }
                    
                     
                 }
               
             })

            
        },
         addCharts(){
                let ctx = document.getElementById("myChart").getContext("2d")
                let data = [9,15]
              var myDoughnutChart = new Chart(ctx, {
                    type: 'doughnut',
                    data: data,
                    options: options
                });
         }                
}
}

</script>

<style>

</style>
