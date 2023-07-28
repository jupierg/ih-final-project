<template>
  <Nav />
  <!-- <VCalendar /> -->
  <div class="calendar-style">
  <h2>Check the calendar to organize yourself!</h2>
  <VDatePicker v-model="date" />
</div>
</template>

<script setup>
import Nav from '../components/Nav.vue';
</script>
<style>
.calendar-style {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 2%;
}

h2 {
  margin: 2.5% 4% 5% 4%;
  text-align: center;
  font-size: 1.5rem;
}

@media (max-width: 768px){
.calendar-style {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 5%;
}

h2 {
  margin: 2.5% 4% 12% 4%;
  text-align: center;
  font-size: 1rem;
}
}

</style>
