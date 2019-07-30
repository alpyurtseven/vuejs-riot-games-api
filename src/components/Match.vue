<template>
    <div class="row">
        
             <div class="col-md-2" style="background-color:#41b883">
                    Yurtseven
             </div>
             <div class="col-md-10" style="background-color:#35495e;border-bottom:1px solid #118df2">
                <div class="row">
                    <div class="col-md-2" >
                            <div class="row">
                                <div  class="col-md-12 mt-2" >
                                      <img   :src="info[0].ico" alt="İcon" style="width:100px;height:100px;margin-left:40px">
                                </div>
                            </div>
                            <div class="row ">
                                <div class="col-md-12">
                                     <div class="container d-flex justify-content-center " style="color:White">
                                         <p>{{info[0].name}}<br> Level:{{info[0].level}}</p>
                                         
                                     </div>
                                </div>
                            </div>
                    </div>
                     <div class="col-md-10" style="border-left: 1px solid #41b883" >
                              <div class="row" style="border-bottom: 1px solid #41b883"> 
                                <div  class="col-md-12" >
                                    <div class="container d-flex justify-content-center row " style="color:white">
                                       <div class="col-md-3">
                                            <canvas id="kda"></canvas>
                                       </div>
                                        <div class="col-md-3">
                                            <canvas id="stats"></canvas>
                                       </div>
                                        <div class="col-md-3 row">
                                            <div class="container mt-4 row">
                                                <div class="col-md-4">
                                                    <img src="../assets/ward.png" alt="" width="35px" height="35px">
                                                </div>
                                                <div class="col-md-8">
                                                     <p>{{visionScore}}</p>
                                                </div>
                                            </div>
                                            <div class="container mt-4 row">
                                                <div class="col-md-4">
                                                    <img src="../assets/minions.png" alt="" width="35px" height="35px">
                                                </div>
                                                <div class="col-md-8">
                                                     <p>{{totalMinionsKilled}}</p>
                                                </div>
                                            </div>
                                       </div>
                                        <div class="col-md-3 mt-1 mb-1">
                                           <div class="row">
                                               <div class="col-md3">
                                                   <img :src="items[0]" alt="">
                                               </div>
                                               <div class="col-md3">
                                                   <img :src="items[1]" alt="">
                                               </div>
                                               <div class="col-md3">
                                                   <img :src="items[2]" alt="">
                                               </div>
                                                <div class="col-md3">
                                                   <img :src="items[7]" alt="">
                                               </div>
                                           </div>
                                            <div class="row">
                                               <div class="col-md3">
                                                   <img :src="items[3]" alt="">
                                               </div>
                                               <div class="col-md3">
                                                   <img :src="items[4]" alt="">
                                               </div>
                                               <div class="col-md3">
                                                   <img :src="items[5]" alt="">
                                               </div>
                                               <div class="col-md3">
                                                   <img :src="items[6]" alt="">
                                               </div>
                                           </div>
                                       </div>
                                       
                                    </div>
                                        
                                </div>
                            </div>
                            <div class="row " >
                                <div class="col-md-12 row">
                                      <div class="container d-flex justify-content-center col-md-12" style="color:#118df2">
                                        <h6 class="col-md-2">Takım 1:</h6>
                                         <p class="col-md-2">{{players[5]}}</p>
                                         <p class="col-md-2">{{players[6]}}</p>
                                         <p class="col-md-2">{{players[7]}}</p>
                                         <p class="col-md-2">{{players[8]}}</p>
                                         <p class="col-md-2">{{players[9]}}</p>
                                         
                                    </div>
                                   
                                     <div class="container d-flex justify-content-center col-md-12 " style="color:#f22b11">
                                          <h6 class="col-md-2">Takım 2:</h6>
                                         <p class="col-md-2">{{players[0]}}</p>
                                         <p class="col-md-2">{{players[1]}}</p>
                                         <p class="col-md-2">{{players[2]}}</p>
                                         <p class="col-md-2">{{players[3]}}</p>
                                         <p class="col-md-2">{{players[4]}}</p>
                                         
                                     </div>
                                </div>
                                
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
            totalDamageDealt : 0 ,
            magicDamageDealt : 0,
            physicalDamageDealt :0 ,
            visionScore:0,
            goldEarned:0,
            goldSpent:0,
            totalMinionsKilled:0,
            champLevel:0,
            items:[],
            assists:0,
            kills:0,
            deaths:0,
            gamesChampion : [],
            gameType : "",
            players : []

        }
  },
  mounted(){
        this.matchdatafetch()
        
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
                        this.totalDamageDealt=  response.data.participants[i].stats.totalDamageDealtToChampions
                        this.magicDamageDealt=response.data.participants[i].stats.magicDamageDealtToChampions
                        this.physicalDamageDealt=response.data.participants[i].stats.physicalDamageDealtToChampions
                        this.visionScore=response.data.participants[i].stats.visionScore
                        this.goldEarned=response.data.participants[i].stats.goldEarned
                        this.goldSpent=response.data.participants[i].stats.goldSpent
                        this.totalMinionsKilled=response.data.participants[i].stats.totalMinionsKilled
                        this.champLevel=response.data.participants[i].stats.champLevel
                     
                        this.items[0]="http://ddragon.leagueoflegends.com/cdn/6.24.1/img/item/"+response.data.participants[i].stats.item0+".png"
                        this.items[1]="http://ddragon.leagueoflegends.com/cdn/6.24.1/img/item/"+response.data.participants[i].stats.item1+".png"
                        this.items[2]="http://ddragon.leagueoflegends.com/cdn/6.24.1/img/item/"+response.data.participants[i].stats.item2+".png"
                        this.items[3]="http://ddragon.leagueoflegends.com/cdn/6.24.1/img/item/"+response.data.participants[i].stats.item3+".png"
                        this.items[4]="http://ddragon.leagueoflegends.com/cdn/6.24.1/img/item/"+response.data.participants[i].stats.item4+".png"
                        this.items[5]="http://ddragon.leagueoflegends.com/cdn/6.24.1/img/item/"+response.data.participants[i].stats.item5+".png"
                        this.items[6]="http://ddragon.leagueoflegends.com/cdn/6.24.1/img/item/"+response.data.participants[i].stats.item6+".png"
                       
                        this.assists=response.data.participants[i].stats.assists
                        this.kills=response.data.participants[i].stats.kills
                        this.deaths=response.data.participants[i].stats.deaths

                        

                     }
                    
                    
                 }
                 this.addCharts()
               
             })

            
        },
         addCharts(){
                let ctx =document.getElementById("kda").getContext("2d")
                let ctx1 =document.getElementById("stats").getContext("2d")
                let labels = ["K","D","A"]
               
                let labels1 = ["K Altın","H Altın","Hasar","Büyü Hasarı","Fiziksel Hasar"]
                let colorHex = ["#253D5B","#fb3640","#F3C310"]
                let colorHex1 = ["#d8f211","#8df211","#f22b11","#f211d8","#f27611"]

               let myChart1 = new  Chart(ctx,{
                    type : "pie",
                    data: {
                        datasets :[{
                            data:[this.kills,this.deaths,this.assists],
                            backgroundColor:colorHex
                        }],
                        labels:labels
                    },
                    options: {
                        responsive : true
                    }
                })
                let myChart2 = new  Chart(ctx1,{
                    type : "line",
                    data: {
                        
                        datasets :[{
                            label:"İstatistik-1",
                            data:[this.goldEarned,this.goldSpent,this.totalDamageDealt,this.magicDamageDealt,this.physicalDamageDealt],
                            backgroundColor:colorHex1
                        }],
                        labels:labels1
                    },
                    options: {
                        responsive : true
                    }
                })
             
         }                
}
}

</script>

<style>

</style>
