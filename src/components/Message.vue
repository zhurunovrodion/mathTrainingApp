<template>

    <div class="alert" :class = "'alert-' + type">
      <h2>{{text}}</h2>
      <button class="btn btn-primary"
          @click="$emit('onNext')"
      >Continue</button>
    </div>


</template>

<script>
  export default {
      props: ['type', 'text']
  }
</script>


<style scoped>
  .alert{
      text-align: center;
  }

  h2, btn{
      margin: 15px 0;
  }

  @media (max-width: 500px ) {
    h2{
      font-size: 1.5rem;
    }


  }

</style>
