<template>
  <div class="login-page">
    <div class="t1"><h1>CHAT</h1></div>
    <div class="loader-container" v-if="isLoading">
      <div class="loader"></div>
    </div>
    <div class="card" v-else>
      <form @submit.prevent="login">
        <div class="title">Login</div>
        <input placeholder="Username" type="text" v-model="username" required />
        <br />
        <input
          placeholder="Password"
          type="password"
          v-model="password"
          required
        />
        <br />
        <button type="submit">Login</button>
      </form>

      <form @submit.prevent="signup">
        <div class="title">Sign Up</div>
        <input
          class="input"
          placeholder="Username"
          type="text"
          required
          v-model="username"
        />
        <br />
        <input
          placeholder="Password"
          type="password"
          v-model="password"
          required
        />
        <br />
        <input placeholder="Email" type="text" v-model="email" required />
        <br />
        <input
          placeholder="First name"
          type="text"
          v-model="first_name"
          required
        />
        <br />
        <input
          placeholder="Last name"
          type="text"
          v-model="last_name"
          required
        />
        <br />
        <button type="submit">Sign Up</button>
      </form>
    </div>
    <div v-if="messageErr" class="snackbar error">
      <span class="message">Verifica tus credenciales de nuevo!</span>
    </div>
  </div>
</template>
    
<script>
import { loginRest, signupRest } from "../ChatAPI/api";

export default {
  data() {
    return {
      username: "",
      password: "",
      email: "",
      first_name: "",
      last_name: "",
      messageErr: false,
      isLoading: false,
    };
  },
  methods: {
    showSnackbarMessage() {
      this.messageErr = true;
      setTimeout(() => {
        this.messageErr = false;
      }, 5000);
    },
    login() {
      this.isLoading = true;
      loginRest(this.username, this.password)
        .then((response) => {
          this.isLoading = false;
          this.$emit("onAuth", { ...response.data, secret: this.password });
        })
        .catch((error) => {
          this.showSnackbarMessage();
          this.isLoading = false;
        });
    },
    signup() {
      this.isLoading = true;
      signupRest(
        this.username,
        this.password,
        this.email,
        this.first_name,
        this.last_name
      )
        .then((response) => {
          this.$emit("onAuth", { ...response.data, secret: this.password });
          this.isLoading = false;
        })
        .catch((error) => {
          this.showSnackbarMessage();
          this.isLoading = false;
        });
    },
  },
};
</script>

<style>
.t1 {
  width: 200px;
  position: relative;
  left: calc(50vw - 100px);
  text-align: center;
  padding-top: 1vw;
  color: #fff;
  text-shadow: 17px 17px 6px rgba(0, 0, 0, 0.89);
}

.login-page {
  width: 100vw;
  height: 100vh;
  background: rgb(0,1,20);
  background: linear-gradient(90deg, rgba(0,1,20,1) 0%, rgba(0,18,75,1) 50%, rgba(0,12,47,1) 100%);
}
.card {
  width: 200px;
  position: relative;
  left: calc(50vw - 100px);
  text-align: center;
}
.title {
  padding-top: 32px;
  font-size: 22px;
  color: white;
  font-weight: 700;
}
input {
  width: calc(100% - 16px);
  margin-top: 12px;
  padding: 8px;
  background-color: #e6f7ff;
  outline: none;
  border: 1px solid #e6f7ff;
  border-radius: 20px;
  border: 1px solid #ccc;
}
input:focus {
  background-color: rgb(187, 240, 255);
  outline: none;
}

button {
  margin-top: 12px;
  width: 100%;
  padding: 8px;
  font-size: 16px;
  background-color: #3498db;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #2079b5;
}

.snackbar {
  position: fixed;
  bottom: 20px;
  right: 20px;
  padding: 10px;
  background-color: #e53935;
  color: #fff;
  border-radius: 4px;
  box-shadow: 0px 2px 5px rgb(255, 255, 255);
  transition: opacity 0.3s ease-in-out;
}

.snackbar.error {
  background-color: #e53935;
}

.loader-container {
  display: flex;
  justify-content: center;
  margin-top: 100px;
  height: 100vh;
}

.loader {
  border: 16px solid #f3f3f3; /* Color del borde del loader */
  border-top: 16px solid #3498db; /* Color de la parte superior del loader */
  border-radius: 50%;
  width: 120px;
  height: 120px;
  animation: spin 2s linear infinite; /* Animación de rotación */
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>