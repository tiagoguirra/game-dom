<template>
  <div id="app">
    <div class="game-show">
      <div class="line" v-for="(line,index) in matriz" :key="index">
        <div class="column" v-for="(col,subindex) in line" :key="subindex">          
          <span class="fire-pixel" :style="{backgroundColor:calculeColor(col)}"></span>
        </div>
      </div>      
      <span style="display:none">{{change}}</span>  
    </div>
    <div class="game-control">      
      <span @click="onMinus">
        <i class="fas fa-minus"></i>
      </span>
      <span @click="onMore">
        <i class="fas fa-plus"></i>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    size: 60,
    matriz: [[]],
    change:0,
    fireColorsPalette:[{"r":7,"g":7,"b":7},{"r":31,"g":7,"b":7},{"r":47,"g":15,"b":7},{"r":71,"g":15,"b":7},{"r":87,"g":23,"b":7},{"r":103,"g":31,"b":7},{"r":119,"g":31,"b":7},{"r":143,"g":39,"b":7},{"r":159,"g":47,"b":7},{"r":175,"g":63,"b":7},{"r":191,"g":71,"b":7},{"r":199,"g":71,"b":7},{"r":223,"g":79,"b":7},{"r":223,"g":87,"b":7},{"r":223,"g":87,"b":7},{"r":215,"g":95,"b":7},{"r":215,"g":95,"b":7},{"r":215,"g":103,"b":15},{"r":207,"g":111,"b":15},{"r":207,"g":119,"b":15},{"r":207,"g":127,"b":15},{"r":207,"g":135,"b":23},{"r":199,"g":135,"b":23},{"r":199,"g":143,"b":23},{"r":199,"g":151,"b":31},{"r":191,"g":159,"b":31},{"r":191,"g":159,"b":31},{"r":191,"g":167,"b":39},{"r":191,"g":167,"b":39},{"r":191,"g":175,"b":47},{"r":183,"g":175,"b":47},{"r":183,"g":183,"b":47},{"r":183,"g":183,"b":55},{"r":207,"g":207,"b":111},{"r":223,"g":223,"b":159},{"r":239,"g":239,"b":199},{"r":255,"g":255,"b":255}]  
  }),
  computed:{
    Matriz(){
      return this.matriz
    }
  },
  methods: {
    calLines() {      
     this.matriz.map((line,lineindex)=>{
       if(lineindex + 1< this.size){         
         line.map((col,colindex)=>{           
           let decay =  Math.floor(Math.random() * 3)
           let decayIndex =  Math.floor(Math.random() * 2)         

           let value = this.matriz[lineindex + 1][colindex - decayIndex] - decay        
           this.matriz[lineindex][colindex] = value>0?(value<36?value:36):0
           this.change = this.change + 1                        
         })    
       }       
     })
    },
    calculeColor(value){
      let color = this.fireColorsPalette[value]
      return `rgb(${color.r},${color.g},${color.b})`
    },
    onMore(){          
      for (let i = 0; i < this.size; i++) { 
        let value =this.matriz[this.size - 1][i] + 1
        this.matriz[this.size - 1][i] = value<36?value:36
      }                
      this.change = this.change + 1      
    },
    onMinus(){      
      for (let i = 0; i < this.size; i++) {
        let value =this.matriz[this.size - 1][i] - 1
        this.matriz[this.size - 1][i] = value>0?value:0
      }      
      this.change = this.change + 1
    }
  },
  created() {
    let matriz = []
    for (let i = 0; i < this.size; i++) {
      let line = [];
      for (let j = 0; j < this.size; j++) {        
        line.push(36);
      }
      matriz.push(line);
    }
    this.matriz = matriz
    setInterval(this.calLines,100)
  },
  mounted() {}
};
</script>

<style>
@import "./assets/global.css";
#app {
  width: 100vw;
  height: 100vh;
  background-color: #000;
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.game-show {
  width: 400px;
  height: 200px;
  display: flex;
  align-items: stretch;
  justify-content: center;
  flex-direction: column;
}
.game-control {
  width: 100%;
  height: 10%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.game-control span {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background-color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 2.5px;
  color:#000;
}
.line {
  flex: 1;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.column {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.fire-pixel{
  width:100%;
  height: 100%;
}
</style>
