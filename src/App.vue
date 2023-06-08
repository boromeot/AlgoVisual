<template class="">
  <div>
    <div>
      <button @click="setCell(0)">Unsearched</button>
      <button @click="setCell(2)">Start</button>
      <button @click="setCell(3)">Wall</button>
      <button @click="setCell(4)">End</button>
      <button @click="bfs(startingCell[0], startingCell[1])">BFS</button>
      <button @click="dfs(startingCell[0], startingCell[1])">DFS</button>
      <button @click="">Dijkstras</button>
      <button @click="removeSearched(startingCell[0], startingCell[1])">Remove Searched</button>
      <button @click="reset()">Reset</button>
    </div>
    <div
      class="grid"
      @mousedown="setIsMouseDown(true)"
      @mouseup="setIsMouseDown(false)"
    >
      <div class="col" v-for="(col, i) in gridArr" :key="i">
        <div
          class="cell"
          :style="{ backgroundColor: map[col[j]] }"
          @click="handleClick(i, j)"
          @mouseover="handleMouseOver(i, j)"
          :key="`${i}-${j}`"
          v-for="(cellValue, j) in col"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        ROWS: 30,
        COLS: 30,
        gridArr: [],
        startingCell: [null, null],
        endingCell: [null, null],
        cell: 0,
        isMouseDown: false,
        map: {
          0: 'whitesmoke',
          1: 'purple',
          2: 'green',
          3: 'black',
          4: 'red'
        }
      };
    },
    mounted() {
      this.createGridArray();
    },
    methods: {
      createGridArray() {
        const tempArr = Array(this.COLS)
          .fill(null)
          .map(() => Array(this.ROWS).fill(0));
        this.gridArr = tempArr;
      },
      handleClick(i, j) {
        if (this.cell === 2) {
          // Check if there is already a start cell
          const [startX, startY] = this.startingCell;
          if (startX !== null && startY !== null) {
            // Remove the existing start cell before setting a new one
            this.gridArr[startX][startY] = 0;
          }
          this.startingCell = [i, j];
        }
        if (this.cell === 4) {
            // Check if there is already a start cell
            const [startX, startY] = this.endingCell;
            if (startX !== null && startY !== null) {
              // Remove the existing start cell before setting a new one
              this.gridArr[startX][startY] = 0;
            }
            this.endingCell = [i, j];
          }
        this.gridArr[i][j] = this.cell;
      },
      handleMouseOver(i, j) {
        if (this.isMouseDown) {
          if (this.cell === 2) {
            // Check if there is already a start cell
            const [startX, startY] = this.startingCell;
            if (startX !== null && startY !== null) {
              // Remove the existing start cell before setting a new one
              this.gridArr[startX][startY] = 0;
            }
            this.startingCell = [i, j];
          }
          if (this.cell === 4) {
            // Check if there is already a start cell
            const [startX, startY] = this.endingCell;
            if (startX !== null && startY !== null) {
              // Remove the existing start cell before setting a new one
              this.gridArr[startX][startY] = 0;
            }
            this.endingCell = [i, j];
          }
          this.gridArr[i][j] = this.cell;
        }
      },
      setCell(value) {
        this.cell = value;
      },
      setIsMouseDown(value) {
        this.isMouseDown = value;
      },
      reset() {
        this.createGridArray();
      },
      removeSearched() {
        for (let i = 0; i < this.ROWS; i++) {
          for (let j = 0; j < this.COLS; j++) {
            if (this.gridArr[i][j] === 1) this.gridArr[i][j] = 0;
          }
        }
      },
      wait(timeout = 18) {
        return new Promise((resolve) => {
          setTimeout(() => {
            resolve();
          }, timeout);
        })
      },
      async bfs(i, j) {
        const DIRECTIONS = [[0, 1], [0, -1], [1, 0], [-1, 0]];
        const q = [[i, j]];
        while (q.length > 0) {
          let [row, col] = q.shift();
          for (let [rd, cd] of DIRECTIONS) {
            let [newR, newC] = [row + rd, col + cd];
            if (newR < this.ROWS && newR >= 0 &&
                newC < this.COLS && newC >= 0) {
                  if (this.gridArr[newR][newC] === 4) {
                    alert('Done');
                    return;
                  }
                  if (this.gridArr[newR][newC] === 0) {
                    await this.wait();
                    this.gridArr[newR][newC] = 1;
                    q.push([newR, newC]);
                  }
              }
          }
        }
      },
      async dfs(i, j) {
        const DIRECTIONS = [[1, 0], [0, 1], [-1, 0], [0, -1]];
        const q = [[i, j]];
        while (q.length > 0) {
          let [row, col] = q.pop();
          for (let [rd, cd] of DIRECTIONS) {
            let [newR, newC] = [row + rd, col + cd];
            if (newR < this.ROWS && newR >= 0 &&
                newC < this.COLS && newC >= 0) {
                  if (this.gridArr[newR][newC] === 4) {
                    alert('Done');
                    return;
                  }
                  if (this.gridArr[newR][newC] === 0) {
                    await this.wait();
                    this.gridArr[newR][newC] = 1;
                    q.push([newR, newC]);
                  }
              }
          }
        }
      },
      async dijkstras(i, j) {
        
      }
    }
  };
</script>

<style>
  .cell {
    width: 25px;
    height: 25px;
    border: 1px solid #ccc;
  }
  .grid {
    user-select: none;
    display: flex;
    flex-wrap: wrap;
  }
</style>
