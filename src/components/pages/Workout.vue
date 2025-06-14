<script setup>
  import { ref, computed } from 'vue';
  import Portal from '../Portal.vue';
  import { workoutProgram, exerciseDescriptions } from '../../utils/utility.js';

  // const selectedWorkout = 10;
  const { data, selectedWorkout, handleSaveWorkout } = defineProps({
    data: Object,
    selectedWorkout: Number, 
    handleSaveWorkout: Function,
    isWorkoutComplete: Boolean
  });
  
  const { workout, warmup } = workoutProgram[selectedWorkout];

  const selectedExercise = ref(null);

  const selectedDescription = computed(() => {
    return selectedExercise.value ? exerciseDescriptions[selectedExercise.value] : '';
  });
  // console.log(warmup);
  const handleCloseModal = () => {
    selectedExercise.value = null;
  };
  // console.log(data[selectedWorkout]['Lat pull down'])
  const workoutType = ['Push', 'Pull', 'Legs'];
</script>

<template>
  <Portal v-if="selectedExercise" :closeModal="handleCloseModal">
    <div class="exercise-description">
      <h4>{{ selectedExercise }}</h4>
      <div>
        <small>Description</small>
        <p>{{ selectedDescription }}</p>
      </div>
      <button @click="handleCloseModal">Close <i class="fa-solid fa-xmark"></i></button>
    </div>
  </Portal>
  <section id="workout-card">
    <div class="plan-card card">
      <div class="plan-card-header">
        <p>Day {{ selectedWorkout < 9 ? '0' + (selectedWorkout + 1) : selectedWorkout + 1 }}</p>
        <i class="fa-solid fa-dumbbell"></i>
      </div>
      <h2>{{ workoutType[selectedWorkout % 3] }} Workout</h2>
    </div>
    <div class="workout-grid">
      <h4 class="grid-name">Warmup</h4>
      <h6>Sets</h6>
      <h6>Reps</h6>
      <h6 class="grid-weights">Weights</h6>
      <div class="workout-grid-row" v-for="(exercise, index) in warmup" :key="index">
        <div class="grid-name">
          <p>{{ exercise.name }}</p>
          <button @click="() => {selectedExercise = exercise.name}"><i class="fa-regular fa-circle-question"></i></button>
        </div>
        <p>{{ exercise.sets }}</p>
        <p>{{ exercise.reps }}</p>
        <input class="grid-weights" placeholder="14kg" type="text" disabled/>
      </div>
      <div class="workout-grid-line"></div>
      <h4 class="grid-name">Workout</h4>
      <h6>Sets</h6>
      <h6>Reps</h6>
      <h6 class="grid-weights">Weights</h6>
      <div class="workout-grid-row" v-for="(exercise, index) in workout" :key="index">
        <div class="grid-name">
          <p>{{ exercise.name }}</p>
          <button @click="() => {selectedExercise = exercise.name}"><i class="fa-regular fa-circle-question"></i></button>
        </div>
        <p>{{ exercise.sets }}</p>
        <p>{{ exercise.reps }}</p>
        <input class="grid-weights" placeholder="14kg" type="text" v-model="data[selectedWorkout][exercise.name]" />
      </div>
    </div>
    <div class="card workout-btns">
      <button @click="handleSaveWorkout">Save and exit <i class="fa-solid fa-save"></i></button>
      <button @click="handleSaveWorkout" :disabled="!isWorkoutComplete">Complete <i class="fa-solid fa-check"></i></button>
    </div>
  </section>
</template>

<style scoped> 
    #workout-card,
    .plan-card{
      display: flex;
      flex-direction: column;
    }
    #workout-card{
      gap: 1rem;
    }
    .plan-card-header{
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .workout-grid,
    .workout-grid-row{
      display: grid;
      grid-template-columns: repeat(7, minmax(0, 1fr));
      gap: 1rem;
    }
    .workout-grid-row,
    .workout-grid-line{
      grid-column: span 7 / span 7;
    }

    .workout-grid-line{
      margin: 0.5rem 0;
      height: 10px;
      border-radius: 2px;
      background: var(--background-muted);
    }

    .grid-name{
      grid-column: span 3 / span 3;
      display: flex;
      align-items: center;
      gap: 1rem;
    }
    .grid-name p{
      text-transform: capitalize;
    }
    .grid-weights{
      grid-column: span 2 / span 2;
    }
    .grid-name button{
      padding: 0;
      border: 0;
      box-shadow: none;
    }
    .grid-name button:hover{
      transform: none;
      box-shadow: none;
      color: var(--color-link);
    }
    .workout-grid-row .grid-name button{
      opacity: 0;
      pointer-events: none; 
    }
    .workout-grid-row:hover .grid-name button{
      opacity: 1;
      pointer-events: all;
    }
    .card.workout-btns{
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 1rem;
    }
    .workout-btns button{
      flex:1;
    }
    .workout-btns button i{
      padding-left: 0.5rem;
    }
    .workout-grid-row p{
      display: flex;
      align-items: center;
    }
    .exercise-description{
      display: flex;
      flex-direction: column;
      gap: 1rem;
      width: 100%;
    }
    .exercise-description h4{
      text-transform: capitalize;
    }
    .exercise-description i{
      padding-left: 0.5rem;
    }
</style>