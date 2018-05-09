<template>
  <div class="training">
    <h1>Math training. Level {{ level+1}}</h1>
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
          :settings="levels[level]"
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
          @nextLevel="onNextLevel"
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
        questMax: 3,
        level: 0,
        levels: [
          {
            from: 10,
            to: 40,
            range: 5,
              variants: 2
          },
          {
            from: 100,
            to: 200,
            range: 20,
            variants: 4
          },
          {
            from: 500,
            to: 900,
            range: 40,
            variants: 6
          },
          {
            from: 1000,
            to: 2000,
            range: 80,
            variants: 10
          },
          {
            from: 10000,
            to: 50000,
            range: 800,
            variants: 15
          },
          {
            from: 100000,
            to: 500000,
            range: 10000,
            variants: 20
          },
          {
            from: 1000000,
            to: 5000000,
            range: 100000,
            variants: 25
          },
          {
            from: 10000000,
            to: 50000000,
            range: 1000000,
            variants: 30
          },
          {
            from: 100000000,
            to: 500000000,
            range: 10000000,
            variants: 35
          },
          {
            from: 1000000000,
            to: 5000000000,
            range: 100000000,
            variants: 40
          },

        ]
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

      },
      onNextLevel(){

        if(this.level+1 < this.levels.length){

          this.level++;
          this.onStart();
        }else{
          this.level = 0;
          this.onStart();
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

  @media (max-width: 500px ) {
    h1{
      font-size: 1.8rem;
    }


  }
</style>
