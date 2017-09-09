<template>
  <div id="top-menu">
    <span v-show="penState" class="title">
     Drawing x: {{ xPos }} y: {{ yPos }}
    </span>
    <span v-show="!penState" class="title">
     Not Drawing x: {{ xPos }} y: {{ yPos }}
    </span>
    
  </div>
</template>
<script>
export default {
  name: "menu",
  props: ['xPos', 'yPos', 'penState', 'wxContext', 'wxCanvas' ],
  data: () => ({
         context: 'wxContext',
         myCanvas: 'wxCanvas'
  }),
  methods: {
    clearCanvas () {
    //  this.context.save()
      this.context.clearRect(0, 0, this.myCanvas.width, this.myCanvas.height)
      this.context.restore()
    }
  }
}
</script>
<style lang="css" scoped>

top-menu {
  height: 50px;
  width: 100vw;
}

</style>
