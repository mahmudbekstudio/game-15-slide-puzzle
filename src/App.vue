<template>
  <div id="app">
    <div class="game-description">Description</div>
    <div class="game">
      <Board :matrix="gameMatrix" @changed="gameChanged"></Board>
    </div>
    <div class="game-panel">
      Pannel<br />
      Time: {{generateTimer(timer)}}<br />
      Moving: {{movingCount}}<br />
      <button v-if="!gameIsStarted" @click="startGame">Start</button>
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
          gameMatrix: [],
          gameIsStarted: false,
          timer: 0,
          movingCount: 0,
          timeHandler: null
      }
    },
    created () {
        this.gameMatrix = this.generateNewMatrix();
    },
    methods: {
        gameChanged (newMatrix) {
            if(!this.gameIsStarted) {
                alert('Please start the game.');
                return false;
            }
            this.movingCount++;
            this.gameMatrix = newMatrix.slice(0);

            if(this.checkWin()) {
                this.gameIsStarted = false;
                clearInterval(this.timeHandler);
                setTimeout(() => {
                    alert('You win!!!');
                }, 0);
            }
        },
        startGame () {
            this.gameIsStarted = true;
            this.timeHandler = setInterval(() => {
                this.timer++;
            }, 1000);
            this.gameMatrix = this.generateNewMatrix(true);
        },
        generateNewMatrix (isRandom = false) {
            let matrix = [];
            for(let i = 1; i <= 15; i++) matrix.push(i);
            matrix.push(0);

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

            if (isRandom) {
                const randCount = 250 + Math.ceil(Math.random() * 50);
                let emptyCell = [3, 3];

                for(var i = 0; i < randCount; i++) {
                    const randResult = this.randomMove(newMatrix, emptyCell);
                    emptyCell = randResult.emptyCell.slice(0);
                    newMatrix = randResult.matrix.slice(0);
                }
            }

            return newMatrix;
        },
        randomMove (matrix, emptyCell) {
            let result = {
                matrix: matrix.slice(0),
                emptyCell: emptyCell.slice(0)
            };
            let movingCells = [];

            if(typeof matrix[emptyCell[0] - 1] !== 'undefined' && typeof matrix[emptyCell[0] - 1][emptyCell[1]] !== 'undefined') {
                movingCells.push([emptyCell[0] - 1, emptyCell[1]]);
            }

            if(typeof matrix[emptyCell[0] + 1] !== 'undefined' && typeof matrix[emptyCell[0] + 1][emptyCell[1]] !== 'undefined') {
                movingCells.push([emptyCell[0] + 1, emptyCell[1]]);
            }

            if(typeof matrix[emptyCell[0]][emptyCell[1] - 1] !== 'undefined') {
                movingCells.push([emptyCell[0], emptyCell[1] - 1]);
            }

            if(typeof matrix[emptyCell[0]][emptyCell[1] + 1] !== 'undefined') {
                movingCells.push([emptyCell[0], emptyCell[1] + 1]);
            }

            var selectedCellIndex = Math.floor(Math.random() * movingCells.length);

            result.matrix[result.emptyCell[0]][result.emptyCell[1]] = result.matrix[movingCells[selectedCellIndex][0]][movingCells[selectedCellIndex][1]];
            result.matrix[movingCells[selectedCellIndex][0]][movingCells[selectedCellIndex][1]] = 0;
            result.emptyCell[0] = movingCells[selectedCellIndex][0];
            result.emptyCell[1] = movingCells[selectedCellIndex][1];

            return result;
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
        },
        generateTimer (s) {
            let hours = Math.floor(s / 60 / 60);
            let minutes = Math.floor((s - hours * 60 * 60) / 60);
            let seconds = s % 60;

            seconds = seconds < 10 ? '0' + seconds : seconds;
            minutes = minutes < 10 ? '0' + minutes : minutes;
            hours = hours < 10 ? '0' + hours : hours;

            return hours + ':' + minutes + ':' + seconds;
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

    .game-description {
      display: inline-block;
      vertical-align: top;
    }
    .game-panel {
      display: inline-block;
      vertical-align: top;
    }
  }
</style>
