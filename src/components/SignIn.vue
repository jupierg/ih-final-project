  <template>

  <div class="container">
    <img class="sign-in-logo" src="../assets/todoverde.png">
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
            :type="passwordVisible ? 'text' : 'password'"
            class="input-field"
            placeholder="**********"
            id="password"
            v-model="password"
            required
          />
        <i class="fa" :class="passwordVisible ? 'fa-eye-slash' : 'fa-eye'" @click="togglePassword('password')"></i>
        </div>
        <button class="button" type="submit"> Log In</button>
    </form>
    <div class="question-account">
    <p>Don't have an account?
    <PersonalRouter :route="route" :buttonText="buttonText" class="sign-link"/></p>
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


const passwordVisible = ref(false);
const confirmPasswordVisible = ref(false);

const togglePassword = (field) => {
  if (field === "password") {
    passwordVisible.value = !passwordVisible.value;
  } else if (field === "confirmPassword") {
    confirmPasswordVisible.value = !confirmPasswordVisible.value;
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

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
  font-size: 1.25rem;
  margin-top: 5%;
  color: black;
}

input {
background-color: #C7D7E4;
border: none;
min-width: fit-content;
padding: 4% 6%;
margin: 2%;
border-radius: 5%;
}

::placeholder {
  color: white;
  font-size: 1rem;
}

.question-account {
margin-top: 7.5%;
font-size: 0.75rem;
display: flex;
flex-direction: column;
align-items: center;
font-size: 1rem;
}

.sign-link:hover {
cursor: pointer;
}

form i {
  margin-left: 95%;
  margin-top: -17.5%;
  cursor: pointer;
  color: white
}

.button:hover {
cursor: pointer;
}

@media (max-width: 768px){
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

.sign-link:hover {
cursor: pointer;
}

form i {
  margin-left: 80%;
  margin-top: -17.5%;
  cursor: pointer;
  color: white
}
};
</style>
