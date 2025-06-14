<script setup>
  import { workoutProgram } from '../utils/utility.js';

  const workoutTypes = [ 'Push', 'Pull', 'Legs'];
  // console.log(workoutProgram)

  const props = defineProps({
    handleSelectWorkout: Function,
    firstIncompleteWorkoutIndex: Number,
    handleResetPlan: Function
  });
  

</script>

<template>
  <section id="grid">
    <button class="card-button plan-card" v-for="(workout, index) in Object.keys(workoutProgram)" :key="index" 
    :disabled="index > 0 && firstIncompleteWorkoutIndex < index" >
      <div @click="handleSelectWorkout(index)">
        <p>Day {{ index < 9 ? '0' + (index + 1) : index + 1 }}</p>
        <i class="fas fa-solid fa-dumbbell" v-if="index % 3 == 0"></i>
        <i class="fas fa-solid fa-weight-hanging" v-if="index % 3 == 1"></i>
        <i class="fas fa-solid fa-bolt" v-if="index % 3 == 2"></i>
      </div>
      <h6>{{ workoutTypes[ index % 3] }}</h6>
    </button>
    <button class="card-button plan-card-reset" @click="handleResetPlan" :disabled="firstIncompleteWorkoutIndex != -1">
      <p>Reset Progress</p>
      <i class="fa-solid fa-rotate-left"></i>
    </button>
  </section>
</template>

<style scoped>
  #grid{
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 20px;
  }
  #grid button{
    width: 100%;
  }
  #grid button:disabled{
    box-shadow: none;
    cursor: not-allowed;
  }
  .plan-card{
    display: flex;
    flex-direction: column;
  }
  .plan-card-reset{
    display: flex;
    align-items: center;
    justify-content: space-around;
    flex-direction: column;
  }
  .plan-card div{
    width: 100%;
    display: flex;
    align-items: baseline;
    justify-content: space-between;
    gap: 1rem;
  }
  .plan-card div p{
    text-align: left;
  }

  @media(min-width: 640px){
    #grid{
      grid-template-columns: repeat(4, minmax(0, 1fr));
    } 
  }
</style>