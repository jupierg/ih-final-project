<template>
  <Nav />
  <div class="user-logo">
  <div>
    <img :src="avatar_url ? avatar_url : 'https://cdn.pixabay.com/photo/2015/10/05/22/37/blank-profile-picture-973460__480.png'" alt="Profile picture">
  </div>
  <div>
     <h1>Login: {{ userEmail }}</h1>
     <h1>Name: {{username}}</h1>
  </div>

  </div>
  <div class="recommend-div">
  <h2 class="recommend">Recommend it to your friends</h2>
  <p class="recommend">Click the link below to send an email recommending our website to your friends:
  <a class="send-mail" href="mailto:?subject=Check%20out%20this%20website&body=Hey%20friends,%20I%20wanted%20to%20share%20this%20awesome%20website%20with%20you:%20https%3A%2F%2Fwww.example.com">Send Email</a>
</p>
</div>
</template>

<script setup>
  import { supabase } from '../supabase'
  import { onMounted, ref, toRefs } from 'vue'
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

</script>

<style>
img {
  width: 200px;
  border-radius: 50%;
}

.user-logo {
  display: flex;
  position: center;
  margin: 5%;
}

.recommend {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 0.8rem;
}

.send-mail:hover {
cursor: pointer;
}

.recommend-div {
display: flex;
flex-direction: column;
margin: 10%;
}

h2 {
  font-size: 1rem;
}
</style>
