<template>
  <v-app dark>
    <v-container fill-height>
      <v-layout row wrap align-center>
        <v-flex xs12 sm8 mx-auto offset-sm2 align-center justify-center>
          <v-card
            class="pa-2 elevation-12 indigo lighten-5">
            <!-- Control bloks start -->
            <v-flex xs12>
              <v-layout justify-center wrap 
                class="indigo darken-1">
                <v-card-text class="text-speed">
                  Текущая скорость: <strong>{{ speed }}</strong>
                </v-card-text>
              </v-layout>
              <app-controls
                :is-running="isRunning"
                @send="delegate($event)"/>
            </v-flex>
            <!-- Control bloks end -->

            <!-- Grid blocks start   -->
            <v-flex xs12>
              <app-grid 
                :message="message"/>
            </v-flex>
            <!-- Grid blocks end   -->
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
import Vue from 'vue';
import Grid from './components/Grid.vue';
import Controls from './components/Controls.vue';

export default {
  name: 'App',
  components: {
    'app-grid': Grid,
    'app-controls': Controls
  },
  data(){
    return {
      isRunning: false,
      message: '',
      speed: 100,
      intervalID: 0
    }
  },
  methods: {
    delegate: function(event){
      if (event === 'play'){
        this.isRunning = !this.isRunning;
        this.restartInterval();
      } else if (event === 'reset'){
        this.updateMessage(event);
      } else if (event === 'speedUp'){
        if (this.speed < 500){
          this.changeSpeed(20);
          this.restartInterval();
        }
      } else if (event === 'speedDown'){
        if (this.speed > 20){
          this.changeSpeed(-20);
          this.restartInterval();
        }
      }
    },
    restartInterval: function() {
      clearInterval(this.intervalID);
      if (this.isRunning) {
        this.intervalID = setInterval(
          this.updateMessage,
          5000 / this.speed,
          'nextStep'
        );
      }
    },
    updateMessage: function(newMessage) {
      this.message = newMessage;
      Vue.nextTick(this.resetMessage);
    },
    resetMessage: function() {
      this.message = '';
    },
    changeSpeed: function(val){
      this.speed += val;
    }
  }
}
</script>

<style>
.text-speed{
  padding: 10px 10px 0 10px;
  text-align: center;
  font-size: 16px;
}
</style>
