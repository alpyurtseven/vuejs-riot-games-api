<template>
<div class="well" >
 
    <div  class="d-flex flex-row bd-highlight mb-3 " style="background-color:#41B883">
        <div v-for="item in info" class="p2 bd highlight" >
            <div class="card ml-2 " style="width: 15rem; background-color:#35495E;color:white" >
            <img class="card-img-top" v-bind:src="item.ico">
                <div class="card-body">
                    <h6 class="card-text">{{item.name}}</h6>
                    <p class="card-text">Level : {{item.level}}</p>
                    
                  
                 
                </div>
            </div>
        </div>
        <Charts class="d-flex flex-row bd-highlight "  :cds="csd" :send="send" v-if="show"/>
       
  </div>
  <hr>
</div>
    
</template>

<script>
import Chart from "chart.js"
import Charts from "./Charts"

export default {
    props : ["info","show"], 
     components:{
        Charts
    },
 
    data(){
        return{
            
            send : [],
            csd : false

        }
    },
    
   watch:{
        show(value){
         
            this.showAll()
        }
   },
    methods: {
      
        showAll(){
            
            if(this.show == true){
                console.log("değişti")
                this.send[0]=(this.info[0]["id"])
                this.send[1]=(this.info[0]["server"]) 


                
                
                
            }
          
        }
    }

  
  
  
}
</script>

<style>

</style>
