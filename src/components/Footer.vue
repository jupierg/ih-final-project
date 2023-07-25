<template>
  <div class="container">
    <h6> © Julieta Gonzalez</h6>
  </div>
</template>

<script setup>
</script>

<style>
*  {
  margin: 0;
  padding: 0;
};

.container {

}

h6 {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  text-align: center;
  background-color: rgba(70, 169, 151, 0.5);
  padding: 20px 0;
  font-size: 1rem;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  color: white;
}

@media (max-width: 768px){
*  {
  margin: 0;
  padding: 0;
};

.container {

}

h6 {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  text-align: center;
  background-color: rgba(70, 169, 151, 0.5);
  padding: 10px 0;
  font-size: 12px;
  color: white;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}
}
</style>
