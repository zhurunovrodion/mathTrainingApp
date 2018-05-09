<template>
  <div class="training">
    <h1>Math training</h1>
    <hr>

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
    ></app-result-screen>

    <div v-else>Unknown state</div>
  </div>
</template>

<script>
  export default {

    data () {
      return {
        state: 'start',
        message:{
          type: '',
          text: ''
        }
      }
    },
    methods:{
      onStart(){
        this.state = 'question';
      },
      onQuestionSuccess(){
        this.state = 'message';
        this.message.text='Good Job';
        this.message.type='success';
      },
      onQuestionError(msg){
        this.state = 'message';
        this.message.text = msg;
        this.message.type='warning';
      },
      onNext(){
        this.state = 'question';
      }
    }
  }
</script>

<style scoped>
 .training{
   max-width: 700px;
   margin: 20px auto;
 }
</style>
