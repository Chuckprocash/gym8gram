<script setup>
    import Grid from '../Grid.vue';
    import { gymHealthFacts } from '../../utils/utility.js';

    const props = defineProps({
        handleSelectWorkout: Function,
        firstIncompleteWorkoutIndex: Number,
    });
    // console.log( gymHealthFacts )
    const randomIndex = Math.floor(Math.random() * gymHealthFacts.length);
    const dailyTip = gymHealthFacts[randomIndex];

</script>
<template>
    <section id="dashboard">
        <div class="card tip-container">
            <h2>Welcome Spidy !!</h2>
            <div class="my-tip">
                <p class="tip"><strong>Daily Tip:</strong><br/>{{ dailyTip }}</p>
            </div>
            <button @click="handleSelectWorkout(firstIncompleteWorkoutIndex)">Start Workout &rarr;</button>
        </div>
        <Grid v-bind="props"/>
    </section>
</template>
<style scoped>
    .my-tip{
        margin-bottom: 1rem;
        margin-top: 0.5rem;
    }
    .tip-container{
        margin-bottom: 1rem;
    }

    @media(min-width:640px){
        .tip-container{
            margin-bottom: 2.5rem;
        }
    }
</style>