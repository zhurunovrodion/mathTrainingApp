<template>
  <div class="alert alert-primary">
    <h3>Success: {{stats.success}}</h3>
    <h3>Errors: {{stats.error}}</h3>
    <h2>Don't delude yourself. Your mamka is fat!</h2>
    <hr>
    <button class="btn btn-primary" @click="$emit('repeat')">Repeat</button>
    <button class="btn btn-primary" @click="$emit('nextLevel')">Next Level</button>
  </div>
</template>

<script>
  export default {
      props:['stats']
  }
</script>


<style>
  @media (max-width: 500px ) {
    h2{
      font-size: 1.55rem;
    }

    h3{
      font-size: 1.45rem;
    }


  }

</style>
