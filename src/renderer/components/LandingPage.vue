<template>
  <div id="wrapper">
  <!--  <wx-menu :x-pos="xPos" :y-pos="yPos" :pen-state="penState"></wx-menu> -->
  <div id="menubar">
    <div class="draggable">
      <span v-show="penState" class="title">
       Drawing x: {{ xPos }} y: {{ yPos }}
      </span>
      <span v-show="!penState" class="title">
       Not Drawing x: {{ xPos }} y: {{ yPos }}
      </span>
    </div>
  <a  @click="clearCanvas()" id="btnClear"><i class="fa fa-square-o fa-2x"></i></a>
<!--  <a  @click="redoPath()" id="btnRedo" disabled>Redo</a> -->
  <a  @click="undoPath()" id="btnUndo" disabled>Undo</a>
  <a  @click="eraser()" id="btnEraser"><i class="fa fa-eraser fa-2x"></i></a>
  <a  @click="highLight()" id="btnHighlight"><i class="fa fa-paint-brush fa-2x"></i></a>
  <a :class="{active:draw}" @click="pen()" id="btnPen"><i class="fa fa-pencil fa-2x"></i></a>
</div>
      <canvas
      @click="startDrawing()"
      @mousedown="draw=true"
      @mouseup="draw=false"
      @mousemove.self="getCoords($event)"
      ref="myCanvas">
    </canvas>
  </div>
</template>

<script>

  import Menu from './Menu.vue'
  export default {
    name: 'landing-page',
    components: { wxMenu: Menu },
    data () {
      return {
        myCanvas: null,
        draw: false,
        xPos: null,
        yPos: null,
        radius: 2,
        context: null,
        windowWidth: null,
        windowHeight: null,
        markerStyle: { color: 'rgba(0,0,0, 1)', radius: 2}
      }
    },
    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      },
      startDrawing () {
        var context = this.context
        this.context.save()
        this.context.canvas.style.cursor = 'crosshair'
        let xPos = this.xPos -5
        let yPos = this.yPos -134
        let radius = this.radius
         context.strokeStyle = this.markerStyle.color
         context.fillStyle = this.markerStyle.color
         context.lineWidth = this.markerStyle.radius * 2
         context.lineTo(xPos, yPos)
         /context.stroke()
         context.beginPath()
         context.arc(xPos , yPos, this.markerStyle.radius, 0, Math.PI*2)
         // context.fill()
         context.beginPath()
         context.moveTo(xPos, yPos)
      },
      getCoords (event) {
        this.xPos = event.pageX
        this.yPos = event.pageY
        this.context.canvas.style.cursor = 'pointer'
        if (this.draw) {
          this.startDrawing()
        }
      },
      clearCanvas () {
        this.context.save()
        this.context.clearRect(0, 0, this.myCanvas.width, this.myCanvas.height)
        this.context.restore()
      },
      handleResize (event) {
        this.windowWidth = window.innerWidth
        this.windowHeight = window.innerHeight
      },
      highLight () {
        this.markerStyle.color = 'rgba(250,3,0, 0.3)'
        this.markerStyle.radius = 40
      },
      pen () {
        this.markerStyle.color = 'rgba(0,0,0, 1)'
        this.markerStyle.radius = 5
      },
      eraser () {
        this.markerStyle.color = 'rgb(252,243,207)'
        this.markerStyle.radius = 50
      },
      undoPath () {
    
      }
    },
    computed: {
      penState () {
        return this.draw
      }
    },
    watch: {
      penState (val) {
        if (val) {
          this.context.beginPath()
        }
      }
    },
    mounted () {
      this.windowWidth = window.innerWidth
      this.windowHeight = window.innerHeight
      var canvas =  this.$refs.myCanvas
      var ctx = canvas.getContext('2d')
      canvas.width = window.innerWidth -300
      canvas.height = window.innerHeight - 100
       ctx.fillStyle = 'rgba(0,0,0,0.0)';
       ctx.lineCap = 'round'
      ctx.fillRect(0,0,window.innerWidth, window.innerHeight);
      // ctx.lineWidth = this.markerStyle.radius * 2
      this.context = ctx
      this.myCanvas = canvas
    },
    ready () {
      window.addEventListener('resize', this.handleResize())
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');
  @import "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css";

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body { font-family: 'Source Sans Pro', sans-serif; }

  html, body {
    background: rgba(0, 0, 0, 0);
}

#wrapper {
  background: rgba(0, 0, 0, 0);
    /*   background:
      radial-gradient(
        ellipse at top left,
        rgba(255, 255, 255, 1) 40%,
        rgba(229, 229, 229, .9) 100%
      ); */
    height: 100vh;
    padding: 0px 0px;
    width: 100vw;
  }

  #logo {
    height: auto;
    margin-bottom: 20px;
    width: 420px;
  }

  .draggable {
    -webkit-app-region: drag;
    position: absolute;
    top: 15px;
    left: 450px;
    width: 300px;
    font-size: 18px;
    cursor: move;
}

#menubar {
  background-color: #fcf3cf;
  padding: 30px 50px;
  margin-right: 300px;
  z-index: 0;
  position: relative;
}

i {
  margin-left: 30px;
}

a:active {
  color: red;
}



canvas {
  background-color:  #fcf3cf;
  margin-top: 0px;
  position: absolute;
  top: 90px;
}
  main {
    display: flex;
    justify-content: space-between;
  }

  main > div { flex-basis: 50%; }

  .left-side {
    display: flex;
    flex-direction: column;
  }

  .welcome {
    color: #555;
    font-size: 23px;
    margin-bottom: 10px;
  }

  .title {
    color: #000;
    font-size: 20px;
    font-weight: bold;
  }

  .title.alt {
    font-size: 18px;
    margin-bottom: 10px;
  }

  .doc p {
    color: black;
    margin-bottom: 10px;
  }

  .doc button {
    font-size: .8em;
    cursor: pointer;
    outline: none;
    padding: 0.75em 2em;
    border-radius: 2em;
    display: inline-block;
    color: #fff;
    background-color: #4fc08d;
    transition: all 0.15s ease;
    box-sizing: border-box;
    border: 1px solid #4fc08d;
  }

  .doc button.alt {
    color: #42b983;
    background-color: transparent;
  }
</style>
