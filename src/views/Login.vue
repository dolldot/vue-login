<template>
  <div class="login">
    <div class="form">
      <img alt="Vue logo" src="../assets/logo.png" />
      <input type="email" name="email" v-model="user.email" placeholder="Email" autocomplete="off" />
      <input type="password" name="password" v-model="user.password" placeholder="Password" />
      <button type="submit" v-on:click="login()">Login</button>
      <p class="register">
        Don't have an account?
        <router-link class="link" to="/register">Sign Up</router-link>
      </p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Login',
  data() {
    return {
      user: {
        email: '',
        password: ''
      }
    };
  },
  methods: {
    login() {
      if (this.user.email != '' && this.user.password != '') {
        axios
          .post('http://localhost:3000/api/v1/auth/login', {
            email: this.user.email,
            password: this.user.password
          })
          .then(async response => {
            let token = response.data.access_token;
            await localStorage.setItem('token', token);
            this.$router.push('/');
          })
          .catch(async err => {
            await localStorage.removeItem('token');
            alert(err.messsage);
          });
      } else {
        alert('Email or Password cannot be empty');
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.login {
  background-color: #ff00ff;
  background-image: url('../assets/katerina-kerdi--YiJvbfNDqk-unsplash.jpg');
  min-width: 100vh;
  min-height: 100vh;
  background-size: cover;
}
.form {
  background-color: rgba(50, 52, 51, 0.5);
  text-align: center;
  width: 30%;
  height: 100vh;
  padding: 20px;
  position: absolute;
  top: 0;
  left: 50%;
  transform: translate(-50%);
}
img {
  margin-bottom: 80px;
}
::placeholder {
  color: white;
  opacity: 0.5;
}
.register {
  margin-top: 20px;
  color: rgba(255, 255, 255, 0.5);
}
.link,
.link:visited {
  color: white;
  text-decoration: none;
}
input[type='email'],
input[type='password'] {
  width: 100%;
  display: block;
  background-color: transparent;
  color: white;
  border: none;
  border-bottom: 1px solid rgba(255, 255, 255, 0.5);
  padding-top: 20px;
  padding-bottom: 20px;
  margin-bottom: 10px;
  box-sizing: border-box;
}
input:focus {
  outline: none;
  border-bottom: 1px solid #41b883;
}
button,
button:visited {
  background-color: #41b883;
  padding: 10px;
  margin-top: 80px;
  width: 100%;
  height: 50px;
  border-radius: 50px;
  border: none;
  padding: 10px;
  color: #35495e;
  font-weight: bold;
  display: inline-block;
  text-transform: uppercase;
  transition: all 0.4s;
  cursor: pointer;
}
button:active,
button:focus {
  outline: none;
  transform: translateY(-1px);
  box-shadow: 0 2.5px 5px rgba(0, 0, 0, 0.2);
}
button:hover {
  transform: translateY(-3px);
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}
</style>
