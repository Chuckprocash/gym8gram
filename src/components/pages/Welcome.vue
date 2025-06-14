<script setup>

  const props = defineProps({
    handleChangeDisplay: {
      type: Function,
      required: true
    }
  });

</script>

<template>
  <section id="welcome">
    <div class="benefits">
      <h2>Complete This Program <br/>If you want to...</h2>
      <ul>
        <li>Follow a simple program with proven results.</li>
        <li>Get fit, healthy, strong and absolutely shreded.</li>
        <li>Learn gym, training and technique.</li>
      </ul>
      <div>
        <h3>The Training Plan</h3>
        <p>This training plan follows a structure known as the <strong>Bro Split</strong>.</p>
        <h5><i>Push &rarr; Pull &rarr; Legs &rarr; Repeat</i></h5>
      </div>
      <div class="card challange">
        <h3>🔥 Your Challange:</h3>
        <p>Complete all the challanges and track your progress along the way</p>
        <p>Do you accept?</p>
        <button @click="handleChangeDisplay(2)">Begin &rarr; </button>
      </div>
    </div>
  </section>
</template>

<style scoped>
  #welcome,
  .challange,
  .benefits{
    display: flex;
    flex-direction: column;
  }
  #welcome{
    gap: 1.5rem;
  }
  .challange{
    gap: 0.25rem;
  }
  .benefits{
    gap: 0.5rem;
  }

  @media(min-width: 640px){
    #welcome{
      gap: 2rem;
      padding: 1rem 0;
    }
    .benefits{
      padding: 1rem;
    }
  }
</style>
