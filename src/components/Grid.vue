<template>
  <v-layout class="column">
    <div 
      @mousedown="isMouseDown = true"
      @mouseup="isMouseDown = false"
      @mouseleave="isMouseDown = false"
      class="grid columns">
      <div
        v-for="(col, indexX) in gridList"
        :key="indexX"
        class="column">
        <app-cell
          v-for="(isAlive, indexY) in col"
          :key="indexY"
          :status-obj="isAlive"
          :is-mouse-down="isMouseDown"
        />
      </div>
    </div>      
  </v-layout>
</template>

<script>
import Cell from './Cell.vue';

export default {
  components: {
    'app-cell': Cell
  },
  props: {
    message: {
      default: '',
      type: String
    }
  },
  data(){
    return {
      width: 46,
      height: 20,
      gridList: [],
      isMouseDown: false
    }
  },
  created(){
    this.initCells();
  },
  watch: {
    message: function(val){
      if (val === 'reset'){
        this.resetGrid();
      } else if (val === 'nextStep'){
        this.update();
      }
    }
  },
  methods:{
    initCells: function(){
      for (let i = 0; i < this.width; i++) {
        this.gridList[i] = [];
        for (let j = 0; j < this.height; j++) {
          this.gridList[i][j] = {isAlive: false};
        }
      }
    },
    resetGrid: function(){
      this.gridList.forEach((col) => {
        col.forEach((cell) => {
          cell.isAlive = false;
        });
      });
    },
    update: function(){
      let tempArr = [];
      for (let i = 0; i < this.width; i++) {
        tempArr[i] = [];
        for (let j = 0; j < this.height; j++) {
          let status = this.gridList[i][j].isAlive;
          let neighbours = this.getNeighbours(i, j);
          let result = false;
          // Rule 1:
          // Any live cell with fewer than two live neighbours dies,
          // as if by under population
          if (status && neighbours < 2) {
            result = false;
          }
          // Rule 2:
          // Any live cell with two or three neighbours lives on
          // to the next generation
          if ((status && neighbours == 2) || neighbours == 3) {
            result = true;
          }
          // Rule 3:
          // Any live cell with more than three live neighbours dies,
          // as if by overpopulation
          if (status && neighbours > 3) {
            result = false;
          }
          // Rule 4:
          // Any dead cell with exactly three live neighbours becomes
          // a live cell, as if by reproduction
          if (!status && neighbours == 3) {
            result = true;
          }

          // Сразу присваивать gridList'y  нельзя, т.к реактивно
          tempArr[i][j] = result;
        }
      }

      for (let i = 0; i < this.width; i++) {
        for (let j = 0; j < this.height; j++) {
          this.gridList[i][j].isAlive = tempArr[i][j];
        }
      }
    },
    getNeighbours: function(posX, posY) {
      let neighbours = 0;
      for (let offsetX = -1; offsetX < 2; offsetX++) {
        for (let offsetY = -1; offsetY < 2; offsetY++) {
          let newX = posX + offsetX;
          let newY = posY + offsetY;
          // check if offset is:
          // on current cell, out of bounds and if isAlive
          // for cell true
          if (
            (offsetX != 0 || offsetY != 0) &&
            newX >= 0 &&
            newX < this.width &&
            newY >= 0 &&
            newY < this.height &&
            this.gridList[posX + offsetX][posY + offsetY].isAlive == true
          ) {
            neighbours++;
          }
        }
      }
      return neighbours;
    }
  }
}
</script>

<style>
.grid {
  border-top: 1px solid #1a0707;
  border-left: 1px solid #1a0707;
  display: flex;
  flex: 1;
  justify-content: center;
  margin-top: 20px;
  background-color: #fff;
}
.column {
  flex: 1;
  display: flex;
  justify-content: center;
  padding: 0;
  margin: 0 auto;
  flex-direction: column;
}
</style>
