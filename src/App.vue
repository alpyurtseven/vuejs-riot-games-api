<template>
  <div id="app">  
      <Search @informationevent="sendinfo($event)" />
      <Content :info="info" :show="show" v-if="click=true" />
      <div class="d-flex justify-content-end w-100" v-if="click=den">
         <Match class="w-75 mt-n3" :info="info" :show="show"  />
         </div>
     
     
  </div>
</template>

<script>
  import Search from "./components/Search"
  import Content from "./components/Content"
  import Match from "./components/Match"
 

export default {
  
  name: 'app',
  data(){
    return{
        den : false,
        click : false,
        name : "",
        level : "",
        ico : "", 
        info : [],
        show : false,

    }
  },
  components: {
    Search,
    Content,
    Match
  
  },
  methods : {
     sendinfo(infos){
        this.info = []
        this.den=true
        this.click=true
        this.name = infos.name
        this.level = infos.level
        this.ico = infos.ico
        this.info.push(infos)
        this.show=this.click
        
        
       

     }
}
}
</script>




<style>

</style>
