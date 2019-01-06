<template>
  <div
    :class="cellClass"
    class="cell"
    @mousedown="seed(true)"
    @mouseover="seed(isMouseDown)"/>
</template>

<script>
export default {
  props: {
    statusObj: {
      default: function() {
        return {isAlive: false};
      },
      type: Object,
    },
    isMouseDown: {
      default: false,
      type: Boolean,
    }
  },
  data(){
    return {
      isAlive: this.statusObj
    }
  },
  methods:{
    seed: function(bool) {
      if (bool){
        this.isAlive.isAlive = !this.isAlive.isAlive;
      }
    }
  },
  computed: {
    cellClass: function(){
      return this.isAlive.isAlive ? 'alive' : 'dead';
    }
  }
}
</script>

<style>
.cell {
  flex: 1;
  border-right: 1px solid #1a0707;
  border-bottom: 1px solid #1a0707;
  padding-bottom: 100%;
  user-select: none;
}
.cell:hover {
  background-color: rgba(60, 255, 0, 0.6);
}
.alive {
  background-color: #47bb47 !important;
}
</style>
