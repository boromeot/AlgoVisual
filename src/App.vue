<template>
  <div>
    <div>
      <button @click="setCell(0)">Unsearched</button>
      <button @click="setCell(2)">Start</button>
      <button @click="setCell(3)">Wall</button>
      <button @click="setCell(4)">End</button>
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
        COLS: 75,
        gridArr: [],
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
        const newGrid = [...this.gridArr];
        newGrid[i][j] = this.cell;
        this.gridArr = newGrid;
      },
      handleMouseOver(i, j) {
        if (this.isMouseDown) {
          const newGrid = [...this.gridArr];
          newGrid[i][j] = this.cell;
          this.gridArr = newGrid;
        }
      },
      setCell(value) {
        this.cell = value;
      },
      setIsMouseDown(value) {
        this.isMouseDown = value;
      }
    }
  };
</script>

<style>
  .cell {
    width: 15px;
    height: 15px;
    border: 1px solid #ccc;
  }
  .grid {
    user-select: none;
    display: flex;
    flex-wrap: wrap;
  }
</style>
