<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col>
            <div class="board">
              <div class="cell" v-for="(cell, cellName) in graph" 
                  :key="cellName" :class="[getCellClass(cell)]" @click="handleUserChoose(cellName)">
              </div>
            </div>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
const TURN_USER = 1
const TURN_COMPUTER = 2

export default {
  name: 'App',

  components: {
  },

  data: () => ({
    graph: {},
    roundCells: [
      {
        x: -1,
        y: -1,
        direction: 1
      },
      {
        x: 0,
        y: -1,
        direction: 2
      },
      {
        x: 1,
        y: -1,
        direction: 3
      },
      {
        x: 1,
        y: 0,
        direction: 4
      },
      {
        x: 1,
        y: 1,
        direction: 5
      },
      {
        x: 0,
        y: 1,
        direction: 6
      },
      {
        x: -1,
        y: 1,
        direction: 7
      },
      {
        x: -1,
        y: 0,
        direction: 8
      }
    ],
    currentTurn: TURN_USER
  }),

  created() {
    this.initGraph()
  },

  methods: {
    initGraph () {
      for (let row = 1; row <= 3; row++) {
        for (let col = 1; col <= 3; col++) {
          let cellName = `${row}-${col}`
          let roundCells = []

          this.roundCells.forEach((cellConfig) => {
            let roundRow = row + cellConfig.y
            let roundCol = col + cellConfig.x


            if (roundRow < 1 || roundRow > 3) {
              return
            }

            if (roundCol < 1 || roundCol > 3) {
              return
            }

            roundCells.push({
              cell: `${roundRow}-${roundCol}`,
              direction: cellConfig.direction
            })
          })

          this.$set(this.graph, cellName, {
            roundCells,
            currentMark: null
          })
        }
      }
    },
    getCellClass (cell) {
      if (cell.currentMark === TURN_USER) {
        return 'cell--user'
      }

      if (cell.currentMark === TURN_COMPUTER) {
        return 'cell--computer'
      }

      return ''
    },
    handleUserChoose (cellName) {
      if (this.currentTurn !== TURN_USER) {
        alert('Not your turn !!!')
        return
      }

      let cell = this.graph[cellName]

      if (cell.currentMark) {
        alert('This cell already chose !')
        return
      }

      this.chooseCell(cellName, cell, TURN_USER)
      this.userChooseCallback()
    },
    chooseCell (cellName, cell, turn) {
      cell.currentMark = turn
    },
    userChooseCallback () {
      this.currentTurn = TURN_COMPUTER
      this.triggerComputerChoose()
    },
    triggerComputerChoose () {

    }
  }
};
</script>

<style lang="scss">
.board {
  width: 450px;
  overflow: hidden;

  .cell {
    width: 150px;
    box-sizing: border-box;
    float: left;
    height: 150px;
    border: 1px solid #000;

    &--user {
      background: blue;
    }

    &--computer {
      background: yellow;
    }
  }
}
</style>
