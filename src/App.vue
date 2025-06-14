<script setup>

  import { ref, computed, onMounted } from 'vue';
  import { workoutProgram } from './utils/utility.js';
  import Layout from './components/layouts/Layout.vue';
  import Welcome from './components/pages/Welcome.vue';
  import Dashboard from './components/pages/Dashboard.vue';
  import Workout from './components/pages/Workout.vue';


  const selectedDisplay = ref(1);
  const selectedWorkout = ref(-1);
  const defaultData = {};
  const data = ref(defaultData);

  for(let workoutid in workoutProgram){
    defaultData[workoutid] = {};
    const workoutData = workoutProgram[workoutid]; 

    for(let exercise of workoutData.workout){
      defaultData[workoutid][exercise.name] = '';
    }
  }

  const isWorkoutComplete = computed(() => {
    const currentWorkout = data.value?.[selectedWorkout.value];
    if (!currentWorkout) return false;
    // console.log(currentWorkout)
    const checkComplete = Object.values(currentWorkout).every(ex => !!ex);
    // console.log('Is Complete: ', checkComplete);
    return checkComplete;
  });

  const firstIncompleteWorkoutIndex = computed(() => {
    const allWorkouts = data.value;
    if(!allWorkouts){return -1}
    for(let [index, workout] of Object.entries(allWorkouts)){
      const isComplete = Object.values(workout).every(ex => !!ex);
      if(!isComplete){return parseInt(index)}
    }
    return -1;
  });

  const handleChangeDisplay = (idx) => {
    selectedDisplay.value = idx;
  };

  const handleSelectWorkout = (workoutId) => {
    selectedWorkout.value = workoutId;
    selectedDisplay.value = 3; // Switch to Workout page
  };

  const handleSaveWorkout = () => {
    localStorage.setItem('workouts', JSON.stringify(data.value));
    selectedDisplay.value = 2; // Switch back to Dashboard
    selectedWorkout.value = -1; // Reset selected workout
    console.log('Workout saved:', data.value);
  };

  const handleResetPlan = () => {
    selectedDisplay = 2;
    selectedWorkout = -1;
    data.value = defaultData;
    localStorage.removeItem('workouts');
  } 

  onMounted(() => {
    if(!localStorage){return}
    if(localStorage.getItem('workouts')){
      const savedData = JSON.parse(localStorage.getItem('workouts'));
      data.value = savedData;
      selectedDisplay.value = 2;
    }
  });
</script>

<template>
  <Layout @changeDisplay="handleChangeDisplay">
    <!-- page1 -->
    <Welcome v-if="selectedDisplay == 1" :handleChangeDisplay="handleChangeDisplay" />
    <!-- page2 -->
    <Dashboard v-if="selectedDisplay == 2" :handleSelectWorkout="handleSelectWorkout" 
    :firstIncompleteWorkoutIndex="firstIncompleteWorkoutIndex" :handleResetPlan="handleResetPlan"/>
    <!-- page3 -->
    <Workout v-if="selectedDisplay == 3 && workoutProgram?.[selectedWorkout]" :isWorkoutComplete="isWorkoutComplete"
    :data="data" :selectedWorkout="selectedWorkout" :handleSaveWorkout="handleSaveWorkout"/>
  </Layout>
</template>

<style scoped>
</style>
