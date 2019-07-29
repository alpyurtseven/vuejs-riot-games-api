<template>
    <div class="p2 bd highlight">


            <div class="  p2 bd highlight" style="width: 25rem;" >
                 <p v-if="show" class="d-flex justify-content-center mt-3">Ranked-Flex</p>
                    <canvas id="myChart"></canvas>
                 <div v-if="show" class="d-flex justify-content-center mt-3">
                       <img  :src="require(`@/assets/${src_flex}`)" width="75" height="75" class="d-inline-block align-top" alt="">
                         <p class="ml-3 mt-4">
                         {{this.flex_rank}}</p>
                    
                    </div>
                  
                    
             </div>
            <div class=" p25 bd highlight" style="width: 25rem;" >
                    <p  v-if="show" class="d-flex justify-content-center mt-3">Ranked-Solo</p>
                    <canvas id="myChart1"></canvas>
                      <div v-if="show" class="d-flex justify-content-center mt-3">
                       <img  :src="require(`@/assets/${src_solo}`)" width="75" height="75" class="d-inline-block align-top" alt="">
                         <p class="ml-3 mt-4">
                         {{this.solo_rank}}</p>
                    
                    </div>
                   
            </div>
            <div class=" p25 bd highlight" style="width: 25rem;"  >
                    <p v-if="show" class="d-flex justify-content-center mt-3">Ranked-TFT</p>
                    <canvas id="myChart2"></canvas>
                    <div v-if="show" class="d-flex justify-content-center mt-3">
                       <img  :src="require(`@/assets/${src_tft}`)" width="75" height="75" class="d-inline-block align-top" alt="">
                         <p class="ml-3 mt-4">
                         {{this.tft_rank}}</p>
                    
                    </div>


                   
                    
                   
            </div>
    </div>
</template>

<script>
import Axios from 'axios';


export default {
    props : ["send","csd"],
    data (){
        return{
       show :false,
        flex_rank:"",
        flex_win:0,
        flex_loses:0,
        solo_rank:"",
        solo_win:0,
        solo_loses:0,
        tft_rank:"",
        tft_win:0,
        tft_loses:0,
        src_flex: "",
        src_tft:"",
        src_solo:""

        
        }
    },
    mounted(){
      this.verial()  
    },
   watch:{
        tft_rank(value){
            this.addCharts()
           
        },
        send(value){
           
        }
       

   },

 methods : {
            addCharts(){
                
                this.show=true
                let ctx = document.getElementById("myChart").getContext("2d")
                let ctx1 = document.getElementById("myChart1").getContext("2d")
                let ctx2 = document.getElementById("myChart2").getContext("2d")
                let labels = ["Kazanma","Kaybetme"]
                let colorHex = ["#253D5B","#fb3640"]

                let myChart = new  Chart(ctx,{
                    type : "doughnut",
                    data: {
                        datasets :[{
                            data:[this.flex_win,this.flex_loses],
                            backgroundColor:colorHex
                        }],
                        labels:labels
                    },
                    options: {
                        responsive : true
                    }
                })
                 let myChart1 = new  Chart(ctx1,{
                    type : "doughnut",
                    data: {
                        datasets :[{
                            data:[this.solo_win,this.solo_loses],
                            backgroundColor:colorHex
                        }],
                        labels:labels
                    },
                    options: {
                        responsive : true
                    }
                })
                   let myChart2 = new  Chart(ctx2,{
                    type : "doughnut",
                    data: {
                        datasets :[{
                            data:[this.tft_win,this.tft_loses],
                            backgroundColor:colorHex
                        }],
                        labels:labels
                    },
                    options: {
                        responsive : true
                    }
                })
                        
            },
            verial(){
                   
                    let url = "https://"+this.send[1]+".api.riotgames.com/lol/league/v4/entries/by-summoner/"+this.send[0]+"?api_key="+this.send[2]
                    Axios.get(url).then(response =>{
                        var i;
                        for(i = 0; i<3; i++){
                           
                                if(response["data"][i]["queueType"]=="RANKED_FLEX_SR"){
                                    this.src_flex=response["data"][i]["tier"]+".png",
                                    this.flex_rank=response["data"][i]["tier"]+response["data"][i]["rank"],
                                    this.flex_win=response["data"][i]["wins"],
                                    this.flex_loses=response["data"][i]["losses"]
                                    
                                
                                }
                                else if(response["data"][i]["queueType"]=="RANKED_TFT"){
                                    
                                    this.src_tft=response["data"][i]["tier"]+".png",
                                    this.tft_rank=response["data"][i]["tier"]+response["data"][i]["rank"],
                                    this.tft_win=response["data"][i]["wins"],
                                    this.tft_loses=response["data"][i]["losses"]
                                   
                                }
                                
                                else if(response["data"][i]["queueType"]=="RANKED_SOLO_5x5"){
                                     this.src_solo= response["data"][i]["tier"]+".png",
                                    this.solo_rank=response["data"][i]["tier"]+response["data"][i]["rank"],
                                    this.solo_win=response["data"][i]["wins"],
                                    this.solo_loses=response["data"][i]["losses"]
                                  
                                }
                               
                               console.log(this.send[0])
                        }
                               
                               
                           
                
                        
                    })
                   




            }
           
             
     },
    
  
}
</script>

<style>

</style>
