<template>
  <div class="polygon">
    <h1>Click and draw you polygon</h1>

    <header class="header">
      <button id="complete" class="btn btn-primary" @click="handleComplete">Complete</button>
      <button id="reset" class="btn btn-secondary" @click="handleReset">Reset</button>
    </header>

    <main class="main">
      <canvas ref="canvas" id="canvas" class="board" @mousedown="addVertex"></canvas>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      canvas: '',
      ctx: '',
      firstVertex: '',
      vertices: []
    }
  },
  mounted() {
    this.canvas = this.$refs.canvas
    this.ctx = this.canvas.getContext('2d')
    this.canvas.width = this.canvas.offsetHeight
    this.canvas.height = this.canvas.offsetHeight

    this.ctx.lineWidth = 2
    this.ctx.strokeStyle = '#337ca0'
    this.ctx.fillStyle = 'orange'
  },
  methods: {
    addVertex(e) {
      this.vertices.push({ x: e.offsetX, y: e.offsetY })
      this.draw()
    },
    draw() {
      if (this.vertices.length === 1) {
        this.firstVertex = this.vertices[0]
        this.ctx.beginPath()
        this.ctx.fillRect(this.vertices[0].x - 2.5, this.vertices[0].y - 2.5, 5, 5) // mark start point
      } else {
        const startVertexLine = this.vertices.shift()
        const endVertexLine = this.vertices[0]
        this.ctx.moveTo(startVertexLine.x, startVertexLine.y)
        this.ctx.lineTo(endVertexLine.x, endVertexLine.y) // draw the line from-to
        this.ctx.stroke()
      }
    },
    handleComplete() {
      this.vertices.push(this.firstVertex) // connect last vertex to first vertex
      this.draw()
    },
    handleReset() {
      this.vertices = []
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height)
    }
  }
}
</script>

<style lang="scss" scoped>
$start-point-color: 'orange';
$line-color: #337ca0;
$btn-complete-color: #3ec300;
$btn-reset-color: #e13700;
$white-color: #fff;
$board-color: #f1f1f1;

.polygon {
  display: flex;
  flex-direction: column;
  place-items: center;

  .header {
    width: 80vw;
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    margin-bottom: 5vh;

    .btn {
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
      color: $white-color;
      font-size: 1.1rem;
      cursor: pointer;

      &.btn-primary {
        background-color: $btn-complete-color;
      }

      &.btn-secondary {
        background-color: $btn-reset-color;
      }
    }
  }

  .board {
    width: 50vh;
    height: 50vh;
    box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px,
      rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px,
      rgba(0, 0, 0, 0.09) 0px -3px 5px;
    display: block;
    background-color: $board-color;
    cursor: crosshair;
  }

  @media screen and (max-width: 576px) {
    .board {
      width: 80vw;
      height: 80vw;
    }
  }
}
</style>
