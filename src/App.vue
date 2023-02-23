<template>
  <div>
      <b-container fluid>
       <b-row id="infinity_content">
           <b-col cols="12" class="infinity-title">
            <h1 class="text-center text-white">Infinity Anime Scroll</h1>
           </b-col>
       </b-row>
      <b-row id="infinity_content"></b-row>
    </b-container>
  </div>
</template>

<script>

export default {
  name: 'App',
  methods: {
      generateImage(){
        let automaticOption = generateRamdonNumber(2,0)
        //GENERATE AUTOMATIC IMAGE
        switch(automaticOption){
          case 0: {
            let randomTypeAnime = this.animeTypeList[generateRamdonNumber(this.animeTypeList.length, 0)]
            let randomNumerItem = generateRamdonNumber(50, 1)
            let url = `https://cdn-anime-images-api.hisoka17.repl.co/images/${randomTypeAnime}${randomNumerItem}.gif`
            let ouput = {uri: url, altNameAnime: String(randomTypeAnime+randomNumerItem) } 
            return ouput
          }
          case 1:{
            let randomNumerItem = generateRamdonNumber(50, 1)
            return {uri: `https://pic.re/image/${randomNumerItem}`, altNameAnime: 'pic.re random' }
          }
          default: {
            console.log('number automatic errror')
          }
        }
        //GENERATE RAND NUMBER
        function generateRamdonNumber(max, min){
          return Math.floor(Math.random() * (max - min) + min)
        }
      },
      init(){
          //ASSIGN FIRST CONTENT
          let count = 0
          while(count < 16){
            let {uri, altNameAnime} = this.generateImage()
            this.getContainerInifinity().innerHTML += `
             <div class="col-12 col-xs-12 col-md-4 col-lg-3 mb-2">
               <div class="infinity-card">
                 <img src="${uri}" alt="${altNameAnime}">
               </div>
             </div>
            `
            count++
          }
      },
      getContainerInifinity(){
        let containerInfinity = document.getElementById('infinity_content')
        return containerInfinity
      },
      infiteScroll(){
      window.addEventListener('scroll', this.checkScrollDown)
      },
      checkScrollDown() {
        let mouseScrollY = window.innerHeight + window.pageYOffset
        let bodyScrollY = document.querySelector('#app').offsetHeight
        if(mouseScrollY >= bodyScrollY){
            debounce(()=>{ this.init()}, 3000)
        }
        
        function debounce (func, delay) {
          //CREATE TMP DIV
          let tmpDiv = document.createElement('div')
          tmpDiv.classList.add('infinity-loading')
          tmpDiv.setAttribute("id", "infinity_loading")
          document.body.appendChild(tmpDiv)

          let debounceTimer = 0
          setInterval(()=>{
            if(debounceTimer == 1){
              //CLOSE TIME AND DELETE TMP DIV
              clearInterval(debounceTimer);
              let getTmpDiv = document.querySelector('#infinity_loading')
              getTmpDiv.remove()
              func.apply()
            }
            debounceTimer++
          }, delay)
        }
      },
  },
  data() {
    return {
      animeTypeList: ['hug','kiss','cuddle']
    }
  },
  mounted() {
    this.init()
    this.infiteScroll()
  },
}
</script>

<style>
body::-webkit-scrollbar, pre::-webkit-scrollbar {
    width: 5px;
    background: #FFFFFF;
    height: 5px;
}
body::-webkit-scrollbar-thumb, pre::-webkit-scrollbar-thumb {
    background: #000;
    border-radius: 5px;
}
.infinity-card{
  height: 300px;
  width: auto;
}
.infinity-card img{
  width: 100%;
  height: 100%;
}
.infinity-title{
  background: #00B4DB;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #0083B0, #00B4DB);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #0083B0, #00B4DB); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  position: fixed;
  top: 0;
  box-shadow: 8px 8px 5px 0px rgba(0,0,0,0.49);
  -webkit-box-shadow: 8px 8px 5px 0px rgba(0,0,0,0.49);
  -moz-box-shadow: 8px 8px 5px 0px rgba(0,0,0,0.49);
}
.infinity-loading{
  background: #000;
  opacity: 0.7;
  position: fixed;
  top: 0;
  width: 100%;
  height: 100vh;
  z-index: 1;
}
.infinity-loading::after{
  content: '';
  border-top: 25px solid #fff;
  border-bottom: 25px solid #0083B0;
  border-left: 25px solid transparent;
  border-right: 25px solid transparent;
  position: absolute;
  left: 50%;
  top: 50%;
  animation: loading 4s ease-in-out infinite;
}

@keyframes loading {
  0% {
    transform: rotate(0deg);
  }
  250%{
    transform: rotate(900deg);
  }
  50%{
    transform: rotate(180deg);
  }
  70%{
    transform: rotate(360deg);
  }
  100% {
    transform: rotate(0deg);
  }
}
</style>
