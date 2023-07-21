  <template>

  <div class="container">
    <img class="sign-in-logo" src="../assets/todoverde.png">
    <!-- <h3 class="header-title">Log In</h3> -->
    <form @submit.prevent="signIn" class="form-sign-in">
        <div class="form-input">
          <label class="input-field-label"></label>
          <input
            type="email"
            class="input-field"
            placeholder="example@mail.com"
            id="email"
            v-model="email"
            required
          />
        </div>
        <div class="form-input">
          <label class="input-field-label"></label>
          <input
            type="password"
            class="input-field"
            placeholder="**********"
            id="password"
            v-model="password"
            required
          />
        </div>
        <button class="button" type="submit"> Log In</button>
    </form>
    <div class="question-account">
    <p>Don't have an account?
    <PersonalRouter :route="route" :buttonText="buttonText" class="sign-up-link"/></p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import PersonalRouter from "./PersonalRouter.vue";

// Route Variables
const route = "/auth/signup";
const buttonText = "Sign Up";

// Input
const email = ref("");
const password = ref("");

// Error
const errorMessage = ref ("");

const routerRedirect = useRouter();



// Arrow function to Signin user to supaBase

const signIn = async () => {
  try {
    await useUserStore().signIn(email.value, password.value);
    routerRedirect.push({ path: "/" });
  } catch (error) {
    alert("Error")
  }
};

errorMessage.value = "error";

</script>

<style>
.sign-in-logo {
  margin-top: 5%;
  max-height: 10%;
}

.container {
background-color: white;
display: flex;
flex-direction: column;
align-items: center;
font-family: Helvetica, Arial;
}

.form-sign-in {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.button {
  font-family: Helvetica, Arial;
  font-weight: 600;
  font-size: 1rem;
  color: black;
}

input {
background-color: #C7D7E4;
border: none;
padding: 4% 6%;
margin: 2%;
border-radius: 5%;
}

::placeholder {
  color: white;
}

.question-account {
margin-top: 10%;
font-size: 0.75rem;
display: flex;
flex-direction: column;
align-items: center;
}

.sign-up-link:hover {
cursor: pointer;
}
</style>
