<template>

  <div id="wrapper app">
    <div class="container">
      <main>
        <h1>Vue</h1>
        <div v-if="authUser">
          <h2>Signed in as {{ authUser.email }}</h2>
          <img :src="authUser.photoURL" height="150">
          <button @click="signOut">Sign Out</button>
        </div>

        <div v-else>
          <form @submit.prevent="register">
            <h2>Register</h2>
            <input type="email" v-model="email" placeholder="Enter your email here..">
            <input type="password" v-model="password" placeholder="*****">
            <button>Register</button>
          </form>

          <form @submit.prevent="signIn">
            <h2>Sign In</h2>
            <input type="email" v-model="email" placeholder="Enter your email here..">
            <input type="password" v-model="password" placeholder="*****">
            <button>Sign In</button>
          </form>

          <div>
            <h2>Sign in with Google</h2>
            <button @click="signInWithGoogle">Sign In</button>
          </div>
        </div>
      </main>
    </div>
  </div>

  <!-- <div id="wrapper">
    <img id="logo" src="~@/assets/logo.png" alt="electron-vue">
    <main>
      <div class="left-side">
        <span class="title">
          Welcome to your new proaerwerwerwawerject!
        </span>
        <system-information></system-information>
      </div>

      <div class="right-side">
        <div class="doc">
          <div class="title">Getting Started</div>
          <button @click="open('https://simulatedgreg.gitbooks.io/electron-vue/content/')">Read the Docs</button><br><br>
        </div>
        <div class="doc">
          <div class="title alt">Other Documentation</div>
          <button class="alt" @click="open('https://electron.atom.io/docs/')">Electron</button>
          <button class="alt" @click="open('https://vuejs.org/v2/guide/')">Vue.js</button>
        </div>
      </div>
    </main>
  </div> -->
</template>
<script src="https://www.gstatic.com/firebasejs/5.5.3/firebase.js"></script>
  <script>
  // Initialize Firebase
  var config = {
    apiKey: "AIzaSyBREk-iZ8XS0OU_qaJ94QCxTOmaupAQbLw",
    authDomain: "reward-3d697.firebaseapp.com",
    databaseURL: "https://reward-3d697.firebaseio.com",
    projectId: "reward-3d697",
    storageBucket: "reward-3d697.appspot.com",
    messagingSenderId: "585796887176"
  };
  firebase.initializeApp(config);

  new Vue({
    el: "#app",

    data: {
      name: 'loginpage',
      email: "",
      password: "",
      authUser: null
    },
    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      },
      register() {
        firebase
          .auth()
          .createUserWithEmailAndPassword(this.email, this.password)
          .then(user => {
            this.authUser = user;
          })
          .catch(error => alert("Error: " + error.message));
      },
      signOut() {
        firebase.auth().signOut();
      },
      signIn() {
        firebase
          .auth()
          .signInWithEmailAndPassword(this.email, this.password)
          .then(user => {
            this.authUser = user;
          })
          .catch(error => alert("Error: " + error.message));
      },
      signInWithGoogle() {
        const provider = new firebase.auth.GoogleAuthProvider();
        firebase
          .auth()
          .signInWithPopup(provider)
          .catch(error => alert("Error: " + error.message))
          .then(data => console.log(data.user, data.credential.accessToken));
      }
    },

    created() {
      firebase.auth().onAuthStateChanged(user => {
        this.authUser = user;
      });
    }
  });

  // firebase.auth().onAuthStateChanged(user => {})
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Source+Sans+Pro");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Source Sans Pro", sans-serif;
}

#wrapper {
  background: radial-gradient(ellipse at top left,
    rgba(255, 255, 255, 1) 40%,
    rgba(229, 229, 229, 0.9) 100%);
  height: 100vh;
  padding: 60px 80px;
  width: 100vw;
}

#logo {
  height: auto;
  margin-bottom: 20px;
  width: 420px;
}

main {
  display: flex;
  justify-content: space-between;
}

main>div {
  flex-basis: 50%;
}

.left-side {
  display: flex;
  flex-direction: column;
}

.welcome {
  color: #555;
  font-size: 23px;
  margin-bottom: 10px;
}

.title {
  color: #2c3e50;
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 6px;
}

.title.alt {
  font-size: 18px;
  margin-bottom: 10px;
}

.doc p {
  color: black;
  margin-bottom: 10px;
}

.doc button {
  font-size: 0.8em;
  cursor: pointer;
  outline: none;
  padding: 0.75em 2em;
  border-radius: 2em;
  display: inline-block;
  color: #fff;
  background-color: #4fc08d;
  transition: all 0.15s ease;
  box-sizing: border-box;
  border: 1px solid #4fc08d;
}

.doc button.alt {
  color: #42b983;
  background-color: transparent;
}
</style>
