<template>
  <div class="training">
    <h1>Math training</h1>
    <hr>
    <div class="progress">
      <div class="progress-bar" :style="progressStyles"></div>
    </div>
    <div class="box">
      <transition name="flip" mode="out-in">
        <app-start-screen
          v-if="state == 'start'"
          @onStart="onStart"

        ></app-start-screen>

        <app-question
          v-else-if="state == 'question'"
          @success="onQuestionSuccess"
          @error="onQuestionError"
        ></app-question>

        <app-message
          v-else-if="state == 'message'"
          :type = "message.type"
          :text = "message.text"
          @onNext="onNext"
        ></app-message>

        <app-result-screen
          v-else-if="state == 'result'"
          :stats="stats"
          @repeat="onStart"
        ></app-result-screen>

        <div v-else>Unknown state</div>
      </transition>

    </div>
  </div>
</template>

<script>
  export default {

    data () {
      return {
        state: 'start',
        stats: {
          success: 0,
          error: 0,
        },
        message:{
          type: '',
          text: ''
        },
        questMax: 3
      }
    },
    computed:{
        questDone(){
          return this.stats.success + this.stats.error;
        },
      progressStyles(){
          return{
            width: (this.questDone / this.questMax * 100) + '%'
          }
      }

    },
    methods:{
      onStart(){
        this.state = 'question';
        this.stats.success = 0;
        this.stats.error =0;
      },
      onQuestionSuccess(){
        this.state = 'message';
        this.message.text='Good Job, but you are vse ravno pidor';
        this.message.type='success';
        this.stats.success++;
      },
      onQuestionError(msg){
        this.state = 'message';
        this.message.text = msg;
        this.message.type='warning';
        this.stats.error++;
      },
      onNext(){
        if(this.questDone < this.questMax){
          this.state = 'question';
        }else{
          this.state = 'result';
        }

      }
    }
  }
</script>

<style scoped>

  .box{
    margin-top: 15px;

  }
 .training{
   max-width: 700px;
   margin: 20px auto;
 }

  .flip-enter{

  }

 .flip-enter-active{
    animation: flipInX 0.3s linear;
 }

 .flip-leave{

 }

 .flip-active{
   animation: flipOutX 0.3s linear;
 }

  @keyframes flipInX {
    from{transform: rotateX(90deg);}
    to{transform: roteteX(0deg);}
  }

 @keyframes flipOutX {
   from{transform: rotateX(0deg);}
   to{transform: roteteX(90deg);}
 }
</style>
