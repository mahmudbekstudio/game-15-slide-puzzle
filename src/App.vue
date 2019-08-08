<template>
  <div id="app">
    <div class="game">
      <Board :matrix="gameMatrix" @changed="gameChanged"></Board>
    </div>
  </div>
</template>

<script>
import Board from './components/Board.vue'
import config from './config';

export default {
  name: 'app',
    data () {
      return {
          gameMatrix: []
      }
    },
    created () {
        this.gameMatrix = this.generateNewMatrix();
    },
    methods: {
        gameChanged (newMatrix) {
            this.gameMatrix = newMatrix.slice(0);

            if(this.checkWin()) {
                setTimeout(() => {
                    alert('You win!!!');
                }, 0);
            }
        },
        generateNewMatrix () {
            let matrix = [];
            for(let i = 1; i <= 15; i++) matrix.push(i);
            matrix.push(0);
            /*matrix.sort(function() {
                return .5 - Math.random();
            });*/

            let newMatrix = [];
            let matrixK = 0;

            for (let i = 0; i < matrix.length; i++) {
                if (i > 0 && i % 4 === 0) {
                    matrixK++;
                }

                if (typeof newMatrix[matrixK] === 'undefined') {
                    newMatrix[matrixK] = [];
                }

                newMatrix[matrixK].push(matrix[i]);
            }

            return newMatrix;
        },
        checkWin () {
            let kNum = 1;

            for (let i = 0; i < this.gameMatrix.length; i++) {
                for (let k = 0; k < this.gameMatrix[i].length; k++) {
                    if(kNum <= 15 && this.gameMatrix[i][k] !== kNum) {
                        return false;
                    }
                    kNum++;
                }
            }

            return true;
        }
    },
  components: {
      Board
  }
}
</script>

<style lang="scss">
  #app {
    text-align: center;

    .game {
      display: inline-block;
      margin: 0 auto;
    }
  }
</style>
