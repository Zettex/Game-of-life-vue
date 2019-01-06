<template>
  <v-layout justify-center wrap class="indigo darken-1" pa-2>
    <v-btn
      :class="btnPlayClass"
      @click="send('play')">
        {{ btnPlayText }}
    </v-btn>
    <v-btn
      @click="send('reset')">
      Сбросить
    </v-btn>
    <v-btn
      @click="send('speedUp')">
      Увеличить скорость
    </v-btn>
    <v-btn
      @click="send('speedDown')">
      Уменьшить скорость
    </v-btn>
  </v-layout>
</template>

<script>
export default {
  props:{
    isRunning: {
      default: false,
      type: Boolean
    }
  },
  data(){
    return{

    }
  },
  methods: {
    send: function(event){
      this.$emit('send', event);
    }
  },
  computed: {
    btnPlayClass: function() {
      return this.isRunning ? "red darken-3" : "grey darken-4";
    },
    btnPlayText: function() {
      return this.isRunning ? "Стоп" : "Старт";
    }
  }
}
</script>

<style>
.v-btn{
  width: 205px;
}
</style>
