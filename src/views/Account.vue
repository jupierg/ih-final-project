<template>
  <Nav />
  <div class="user-logo">
      <img class="profile-img" :src="avatar_url ? avatar_url : 'https://cdn.pixabay.com/photo/2015/10/05/22/37/blank-profile-picture-973460__480.png'" alt="Profile picture">
      <h1>Login: {{ userEmail }}</h1>
  </div>
  <section id="contact-main">
        <div class="contact-container">
          <h1 class="contact">Contact Us</h1>
          <div id="contact-form" class="form-container">
            <form id="form-message" action="">
              <label for="name">Full Name *</label>
              <input id="name" type="text" placeholder="Enter your full name" required="">
              <div class="email-phone">
                <div id="email">
                  <label for="email">Email</label>
                  <input id="email" type="email" placeholder="Enter your email" required>
                </div>
                <div id="phone">
                  <label for="phone">Phone</label>
                  <input id="phone" type="text" placeholder="Enter your phone">
                </div>
              </div>
              <label for="message">Message *</label>
              <textarea placeholder="Write your message here..." maxlength="5000" id="message" required></textarea>
              <button type="submit" class="add-task" id="submit">Submit</button>
            </form>
          </div>
        </div>
    </section>

  <div class="recommend-div">
    <p class="recommend"> Recommend todo App </p>
     <a class="send-mail" href="mailto:?subject=Check%20out%20this%20website&body=Hey%20friends,%20I%20wanted%20to%20share%20this%20awesome%20website%20with%20you:%20https%3A%2F%2Fwww.example.com"><img src="../assets/mail.png" alt="Mail icon"></a>
  </div>

</template>

<script setup>
  import { supabase } from '../supabase'
  import { onMounted, ref, reactive, toRefs } from 'vue'
  import { useUserStore } from "../stores/user";
  import Nav from '../components/Nav.vue';

  const userStore = useUserStore();

  const loading = ref(false);
  const username = ref(null);
  const website = ref(null);
  const avatar_url = ref(null);

  onMounted(() => {
    getProfile();
  });

  async function getProfile() {
    await userStore.fetchUser();
    username.value = userStore.profile.username;
    avatar_url.value = userStore.profile.avatar_url;
  }

  async function signOut() {
    try {
      loading.value = true
      let { error } = await supabase.auth.signOut()
      if (error) throw error
    } catch (error) {
      alert(error.message)
    } finally {
      loading.value = false
    }
  }

  const getUser = useUserStore().user;
  const userEmail = getUser.email;

  const nametext = ref("");
  const motivation = ref("");

  const updateInfo = () => {
  motivation.value = nametext.value;
  nametext.value = "";
};

</script>

<style>
.profile-img {
  width: 150px;
  border-radius: 50%;
}

h1 {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 1rem;
}

label {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

.user-logo {
  display: flex;
  position: center;
  margin: 2% 15%;
  align-items: center;
}

.contact-container > h1 {
  margin: 2% 5% 2% 5%;
  font-size: 2rem;
}

#form-message {
  display: flex;
  flex-direction: column;
  margin: 2% 10%;
}

#form-message > label {
  color:#6e738c;
  font-size: 1rem;
}

#form-message > input {
  background-color: #6b708d1a;
  border: none;
  font-size: 1rem;
  height: 50px;
  margin-bottom: 1rem;
  padding: 10px 10px;
}

#form-message > textarea {
  background-color: #6b708d1a;
  border: none;
  font-size: 1rem;
  height: 200px;
  margin-top: 1rem;
  margin-bottom: 1rem;
  padding: 10px 10px;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

.email-phone {
  display: flex;
  flex-direction: space-between;
  gap: 12.5px;
}

.email-phone > label {
  color:#6e738c;
  font-size: 1rem;
  margin-bottom: 1rem;
}

.email-phone > input {
  background-color: #6b708d1a;
  border: none;
  font-size: 1rem;
  height: 50px;
  margin-bottom: 1rem;
  padding: 7px 7px;
}

#email {
  width: 90%;
}

#phone {
  width: 90%;
}

#email, #phone {
  display: flex;
  flex-direction: column;
}

#email > label {
  color:#6e738c;
  font-size: 1rem;
  margin-bottom: 1rem;
}

#email > input {
  background-color: #6b708d1a;
  border: none;
  font-size: 1rem;
  height: 50px;
  margin-bottom: 1rem;
  padding: 7px 7px;
}

#phone > label {
  color:#6e738c;
  font-size: 1rem;
  margin-bottom: 1rem;
}

#phone > input {
  background-color: #6b708d1a;
  border: none;
  font-size: 1rem;
  height: 50px;
  margin-bottom: 1rem;
  padding: 7px 7px;
}

#submit {
  margin-bottom: 8rem;
}

.send-mail:hover {
cursor: pointer;
width: 50;
}

.send-mail img {
width: 50px;
}

h2 {
  font-size: 1rem;
}


.recommend-div {
  display: flex;
  padding: 0% 10%;
  margin-bottom: 2%;
  align-items: center;
  justify-content: center;
}

.recommend {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 1rem;
  align-items: center;
  justify-content: center;
}

@media (max-width: 768px){
.profile-img {
  width: 50px;
  border-radius: 50%;
}

h1 {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 0.8rem;
}

.user-logo {
  display: flex;
  position: center;
  margin: 5% 7.5%;
  align-items: center;
}

.send-mail:hover {
cursor: pointer;
}

.send-mail img {
width: 40px;
}

.contact-container > h1 {
  margin: 30px 0px 35px 0px;
  font-size: 1.5rem;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

#form-message {
  display: flex;
  flex-direction: column;
  margin: 20px 14px 0px 14px;
}

#form-message > label {
  color:#6e738c;
  font-size: 0.85rem;
  margin-bottom: 1rem;
}

#form-message > input {
  background-color: #6b708d1a;
  border: none;
  font-size: 0.85rem;
  height: 75px;
  margin-bottom: 1.5rem;
  padding: 10px 10px;
}

#form-message > textarea {
  background-color: #6b708d1a;
  border: none;
  font-size: 0.85rem;
  height: 300px;
  margin-bottom: 1.5rem;
  padding: 2% 2%;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

.email-phone {
  display: flex;
  flex-direction: column;
  gap: 0px;
}

.email-phone > label {
  color:#6e738c;
  font-size: 0.85rem;
  margin-bottom: 1.5rem;
}

.email-phone > input {
  background-color: #6b708d1a;
  border: none;
  font-size: 0.85rem;
  height: 75px;
  margin-bottom: 1.5rem;
  padding: 0% 2%;
}

#email {
  width: 100%;
}

#phone {
  width: 100%;
}

#email, #phone {
  display: flex;
  flex-direction: column;
}

#email > label {
  color:#6e738c;
  font-size: 0.85rem;
  margin-bottom: 1.5rem;
}

#email > input {
  background-color: #6b708d1a;
  border: none;
  font-size: 1rem;
  height: 75px;
  margin-bottom: 1.5rem;
  padding: 2% 2%;
}

#phone > label {
  color:#6e738c;
  font-size: 0.85rem;
  margin-bottom: 1.5rem;
}

#phone > input {
  background-color: #6b708d1a;
  border: none;
  font-size: 1rem;
  height: 75px;
  margin-bottom: 1.5rem;
  padding: 2% 2%;
}

.recommend-div {
  display: flex;
  padding: 10% 10%;
  align-items: center;
  justify-content: center;
}

.recommend {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 0.8rem;
  padding: 2%;
}

}
</style>
