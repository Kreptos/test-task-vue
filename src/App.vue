<template>
  <div class="input__wrapper" :class="{active: isArrowActive}">
    <input type="text" name="" id="" v-model="name" class="input" v-on:keyup="search" list="names">
    <img class="arrow" src="./assets/arrow.svg" v-on:click="activate"/>
    <img class="loader" src="./assets/loader2.png" v-if="isSearched"/>
    <div id="names" class="list-name">
      <div class="option" v-for="item in data" v-bind:key="item.id" :value='item.login' v-on:click="selectName(item.login)">{{ item.login }}</div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data(){
    return{
      name: '',
      isArrowActive: false,
      isSearched: false,
      data: [],
      namesList: [],
    }
  },
  methods:{
    activate(){
      this.isArrowActive = !this.isArrowActive
    },
    ifSearching(){
        setTimeout(async () => {
          let f
          let data
          if (this.name){
            f = await fetch(`https://api.github.com/search/users?q=${this?.name}`)
            data = await f?.json();
            this.isArrowActive = true
          }
          else this.isArrowActive = false
          this.data = data?.items
          this.isSearched = false
        }, 1000)
        
    },
    search(){
      if(!this.isSearched){
        this.ifSearching()
        this.isSearched = true
      }
      if(this.isSearched) {
        return
      }
    },
    selectName(newName){
      if(this.name != newName) {
        this.name = newName
        this.isArrowActive = false
      }
    }
  },
}
</script>

<style>
  body{
    background-color: #f5f5f5;
  }
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    font-size: 16px;
    line-height: 20px;
    font-family: Arial, Helvetica, sans-serif;
    box-sizing: border-box;
  }
  .input{
    width: 218px;
    height: 30px;
    font-size: 16px;
    line-height: 20px;
    border: none;
    overflow: hidden;
    outline: none;
    padding-left: 8px;
    padding-right: 32px;
  }
  .input__wrapper{
    background-color: #fff;
    position: relative;
    margin: 0 auto;
    width: 260px;
    max-height: 32px;
    border: 1px solid;
    border-image-slice: 1;
    border-image-source: linear-gradient(to left,#FFC700,#A5E870);
    border-radius: 4px;
  }
  .input__wrapper.active{
    max-height: 152px;
  }

  .input__wrapper.active .option:first-child{
    border-top: 1px solid;
    border-image-slice: 1;
    border-image-source: linear-gradient(to left,#FFC700,#A5E870);
    border-radius: 4px;
  }
  .input__wrapper.active .arrow{
    rotate: 180deg;
  }

  .input__wrapper.active .list-name{
    max-height: 120px;
    width: 260px;
    overflow: hidden;
  }

  .arrow{
    position: absolute;
    right: 10px;
    top: 12px;
    cursor: pointer;
  }

  .loader{
    position: absolute;
    right: 30px;
    top: 8px;
  }

  ol{
    max-height: 0;
    overflow-y: scroll;
  }

  .list-name{
    width: 260px;
    max-height: 0;
    overflow: hidden;
  }
  .option{
    cursor: pointer;
    padding-left: 8px;
    height: 24px;
    vertical-align: middle;
    background: #fff;
    text-align: left;
  }
  .option:hover{
    background-color: #fff6d5;
  }
</style>
