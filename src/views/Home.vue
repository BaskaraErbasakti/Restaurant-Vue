<template>
  <div class="home">
    <div class="sticky-top bg-white">
      <div class="d-flex bg-info">
          <div class="d-flex col-8 justify-content-between align-items-center p-2">
              <img class="menu" src="../assets/menu.png" alt="menu">
              <h1 class="btn fontSize bold" @click="home">Food Items</h1>
              <div class="search d-flex align-items-center rounded-pill">
                <img class="find" src="@/assets/cari.png" alt="search">
                <input class="bg-transparent border-0" type="text" placeholder="Search" v-model="search">
              </div>
          </div>
          <div class="d-flex col-4 justify-content-center align-items-center p-2">
              <h1 class="fontSize">Cart</h1>
              <h1 class="total">{{total}}</h1>
          </div>
      </div>
    </div>
    <contents :total="total" @kurang="minus" @nambah="plus" @tutup="close" @reset="reset" :search="search"></contents>
  </div>
</template>

<script>
// @ is an alias to /src

import contents from "../components/content"

export default {
  name: 'Home',
  components: {
    contents
  },
  data(){
    return {
      total : 0,
      search: '',
    } 
  },
  methods:{
    home(){
      this.$router.go('/')
    },
    minus(){
      if(this.total > 0){
        this.total--
      } 
    },
    plus(){
      this.total++
    },
    close(x){
      this.total -= x
    },
    reset(){
      this.total = 0
    },
  }
}
</script>

<style>
.search{
  padding-left: 1%;
  background-color:#eeefef;
  height: 50%;
}
.home {
  font-family: Airbnb Cereal App;
}
.font-25 {
  font-size: 25px;
}
.tick {
    display: block;
}
.notick {
    display: none;
}
.bold {
    font-weight: 500;
}
.light {
  font-weight: 400;
}
.menu {
    margin: 1% 4%;
}
.total {
    background-color: #57cad5;
    color: white;
    font-size: 170%;
    margin: 0% 2%;
    padding: 0% 1.5%;
    border-radius: 100%;
}
.fontSize {
    font-size: 230%;
}
.find {
  width: 30px;
  height: 30px;
  margin-right: 3%;
}
</style>