<template>
  <div class="game">
      <p v-if="!isDonee">"H" Bul ve Kazan</p>
      <span class="over" v-if="isDonee" >Oyun bitti {{ winner }}</span>
      <span  v-if="turned" class="turn">Sıra 1.de</span> 
      <span  v-if="!turned" class="turn">Sıra 2.de</span>    
      <table cellspacing="0" >
          <tr v-for="(row, rowIndex) in game" v-bind:key="rowIndex">
                <td 
                    v-for="(column, columnIndex) in row" 
                    v-bind:key="columnIndex"
                    v-on:click="click(rowIndex,columnIndex)"
                > 
                    <div id="box" class="dnone" @click="toggle">{{ game[rowIndex][columnIndex] }} </div>
                </td>
          </tr>
      </table>
      <button @click="reset()">Restart</button>
      <div class="score-board">
          <div class="left">
             <span>1. Oyuncu</span>
             <h5>{{ scoreA }}</h5> 
          </div>
          <div class="right">
             <span>2. Oyuncu</span> 
             <h5>{{ scoreB }}</h5>
          </div>          
      </div>
      <div class="select">
          <button @click="changeone()">1.Oyuncu</button>
          <button  @click="changetwo()">2.Oyuncu</button>
      </div>
  </div>
</template>

<script>
export default {
    name: 'Game',
    data() {
        return {
            game: [
                ['','','',''],
                ['','','',''],
                ['','','',''],
                ['','','',''],
            ],  
            isDonee: false,   
            turn: 0,
            turned: true,  
            winner: null,   
            scoreA:0,
            scoreB:0, 
        }
    },
    methods: {
        toggle: function(event) {
            if(this.isDonee) {                
                return
            }

            event.target.classList.remove("dnone");

            this.turn++;
            //console.log(this.turn)

            if(this.turn % 2 == 0){
                this.winner = "2. Oyuncu Kazandı";                
            }else if(this.turn % 2 == 1){
                this.winner = "1. Oyuncu Kazandı"                
            }  

            if(this.turn % 2 == 1) {
                this.turned = false;
            }else {
                this.turned = true;
            }
            

        },
        click: function click(x,y) {
            if(this.isDonee) {
                return
            }

            if(this.game[x][y] === "H") {
                this.isDonee = true                
                
            }
                     
            if(this.isDonee) {
                if(this.winner === "1. Oyuncu Kazandı") {
                    this.scoreA += 1;
                    this.counter++
                    console.log(this.counter)
                }else if (this.winner === "2. Oyuncu Kazandı"){
                    this.scoreB += 1;
                    this.counter++
                    console.log(this.counter)
                }
            }
        
            
            this.$forceUpdate();
            
        },
        reset: function reset() {
            const randomX = Math.floor(Math.random() * 4);
            const randomY = Math.floor(Math.random() * 4);
            

            for(let i = 0; i < 4; i++){
                for(let k = 0; k < 4; k++){
                    this.game[i][k] = "O";

                    
                }
            }

            document.querySelectorAll("#box").forEach(enumeration => {
                enumeration.classList.add("dnone");
            });

            this.game[randomX][randomY] = 'H';   
            this.isDonee = false;
            //this.turn= 0;

            this.$forceUpdate();          
        },
        changeone: function changeone() {
            this.turn = 0;
            if(this.turn % 2 == 1) {
                this.turned = false;
            }else {
                this.turned = true;
            }
        },
        changetwo: function changetwo() {
            this.turn = 1;
            if(this.turn % 2 == 1) {
                this.turned = false;
            }else {
                this.turned = true;
            }
        }

    },
    created: 
         function moveRandom() {

            const randomX = Math.floor(Math.random() * 4);
            const randomY = Math.floor(Math.random() * 4);

            for(let i = 0; i < 4; i++){
                for(let k = 0; k < 4; k++){
                    this.game[i][k] = "O";
                }
            }
            this.game[randomX][randomY] = 'H';   
                          
        },
    
}
</script>

<style>

    .game {
        margin-top: 30px;
        text-align: center;
    }
    table {        
        text-align: center;
        margin: 0 auto;
        margin-top: 30px;
        margin-bottom: 40px;
    }
    .game p {
        font-size: 16px;
        font-weight: normal;
        margin-bottom: 10px;
    }
    .game .turn {
        color: red;
        font-size: 20px;
        background-color: rgb(180, 64, 64);
        color: white;
        padding: 2px 15px;
        margin: 10px;
    }
    .game .over {
        font-size: 22px;
        color: #273c75;
        display: block;
        margin-bottom: 5px;
    }
    .game button {
        width: 100px;
        height: 30px;
        font-size: 18px;
        background-color: #273c75;
        color: #fff;
        cursor: pointer;
    }
    table tr:nth-child(1) td,
    table tr:nth-child(2) td,
    table tr:nth-child(3) td{
        border-bottom: 3px solid rgba(0, 0, 0, .3);
    }
    table tr td:first-child,
    table tr td:nth-child(2),
    table tr td:nth-child(3) {
        border-right: 3px solid rgba(0, 0, 0, .3);
    }
    table tr td {
        width: 80px;
        height: 80px;
        font-size: 28px;
        font-weight: normal;
        line-height: 80px;
        cursor: pointer;
        transition: all .3s ease;
        
    }
    table tr td:hover {
        background-color: #999;
    }
    .dnone {
        opacity: 0;
    }
    .score-board {
        display: flex;
        justify-content: center;
        margin-top: 30px;
    }
    .score-board span {
        color: #000;
        font-size: 18px;
        margin: 10px;
    }
    .score-board h5{
        font-size: 20px;
        margin-top: 10px;
    }
    .select button {
        margin: 5px;
        margin-top: 10px;
        width: 80px;
        font-size: 15px;
        background-color: #273c75;
    }
    .select button:focus {
        background-color: rgb(180, 64, 64);
    }
</style>