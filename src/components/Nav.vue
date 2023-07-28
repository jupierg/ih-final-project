<template>
  <nav>
    <!-- <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link"/> -->
    <router-link to="/" class="navLink">
    <img class="nav-logo" src="../assets/logocincuentablanco.png">
    </router-link>

    <ul>
        <!-- <li>
          <router-link to="/" class="navLink">Task Manager</router-link>
        </li> -->
        <li class="log-out-welcome navLink">
          <p class="welcome">Welcome, <strong> {{ userEmail }} </strong></p>
        </li>
        <li>
          <router-link to="/account" class="navLink"><img class="sign-out-logo" src="../assets/usercincuentablanco.png"></router-link>
        </li>
    </ul>

    <div>
      <ul class="user-log-out">

        <li>
          <!-- <button @click="signOut" class="button">
            <img class="sign-in-logo" src="../assets/logout.png">
          </button> -->
          <img @click="signOut" class="sign-out-logo" src="../assets/logoutcincuentablanco.png">
        </li>
      </ul>
    </div>
  </nav>
</template>

<script setup>
// import PersonalRouter from "./PersonalRouter.vue";
import { useUserStore } from "../stores/user";
import { computed } from "vue";
import { useRouter } from "vue-router";
import { ref } from 'vue';

//constant to save a variable that will hold the use router method
const route = "/";
const buttonText = "Todo app";

// constant to save a variable that will get the user from store with a computed function imported from vue
// const getUser = computed(() => useUserStore().user);
const getUser = useUserStore().user;

// constant that calls user email from the useUSerStore
const userEmail = getUser.email;

// async function that calls the signOut method from the useUserStore and pushes the user back to the Auth view.
const redirect = useRouter();

const signOut = async () => {
  try {
    await useUserStore().signOut();
    redirect.push({ path: "/auth/login"});
    } catch (error) {}
};

</script>

<style>
* {
  margin: 0;
  padding: 0;
}

.nav-logo {
  width: 100px;
}

.navLink {
  color: white;
  text-decoration: none;
  font-size: 0.75rem;
}

nav {
  background-color: white;
  display: flex;
  width: 100%;
  justify-content: space-around;
  align-items: center;
  min-height: 100px;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

nav ul {
  list-style: none;
  padding-inline-start: 0;
  display: flex;
  flex-direction: row;
  align-items: center;
}

/* .user-log-out {
  display: flex;
  flex-direction: column;
} */
.welcome {
  display: visible;
  font-size: 1rem;
  color: rgb(72, 172, 152);
}

.button {
  border-style: none;
  color: rgb(72, 172, 152);
  background-color: white;
  padding: 1rem 2rem;
}

.sign-out-logo {
  max-width:45px;
}

.sign-out-logo:hover {
cursor: pointer
}

@media (max-width: 768px){
* {
  margin: 0;
  padding: 0;
}

.nav-logo {
  width: 100px;
}

.navLink {
  color: white;
  text-decoration: none;
  font-size: 0.75rem;
}

nav {
  background-color: white;
  display: flex;
  width: 100%;
  justify-content: space-around;
  align-items: center;
  max-height: 100px;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

nav ul {
  list-style: none;
  padding-inline-start: 0;
  display: flex;
  flex-direction: row;
  align-items: center;
}

/* .user-log-out {
  display: flex;
  flex-direction: column;
} */
.welcome {
  display:none;
}

.button {
  border-style: none;
  color: rgb(72, 172, 152);
  background-color: white;
  padding: 1rem 2rem;
}

.sign-out-logo {
  max-width:45px;
}

.sign-out-logo:hover {
cursor: pointer
}
};
</style>
