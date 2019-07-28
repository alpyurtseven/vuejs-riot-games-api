<template>
  <div id="app">  
      <Search @informationevent="sendinfo($event)" />
      <Content :info="info" :show="show" v-if="click=true" />
     
  </div>
</template>

<script>
  import Search from "./components/Search"
  import Content from "./components/Content"
 

export default {
  
  name: 'app',
  data(){
    return{
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
  
  },
  methods : {
     sendinfo(infos){
        this.info = []
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
