<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <ul v-for="data in info" :key="data">
      <li> Username: {{data.username}} First Name: {{data.first_name}} Last Name: {{data.last_name}}</li>
      <!-- {{ data.first_name }}
      {{ data.last_name }} -->
    </ul>
    <form method="post" @submit.prevent="checkForm">
      <p v-if="errors.length">
        <b>Please correct the following errors(s):</b>
        <ul>
          <li v-for="error in errors" :key="error">{{error}}</li>
        </ul>
      </p>
      <p>
        <label for="username">Username</label>
        <input type="text" v-model="userForm.username" name="username" id="username">
      </p>
      <p>
        <label for="password">Password</label>
        <input type="password" v-model="userForm.password" name="password" id="password">
      </p>
      <p>
        <label for="firstName">First Name</label>
        <input type="text" v-model="userForm.first_name" name="firstName" id="firstName">
      </p>
      <p>
        <label for="lastName">Last Name</label>
        <input type="text" v-model="userForm.last_name" name="lastName" id="lastName">
      </p>
      <p>
        <label for="age">Age</label>
        <input type="text" v-model="userForm.age" name="age" id="age">
      </p>
      <p><input type="submit" value="Submit"></p>

    </form>
  </div>
</template>

<script>
const axios = require("axios");
const API_URL = "http://localhost:5000/api";
export default {
  name: "HelloWorld",
  data() {
    return {
      errors: [],
      usernameList: [],
      info: null,
      userForm: {
        username: "",
        password: "",
        first_name: "",
        last_name: "",
        age: ""
      }
    };
  },
  props: {
    msg: String
  },
  mounted() {
    axios
      .get(`${API_URL}/users`)
      .then(res => {
        this.info = res.data;
      })
      .catch(error => console.log(error));
  },
  methods: {
    checkForm(e) {
      if (
        this.userForm.username &&
        this.userForm.password &&
        this.userForm.first_name &&
        this.userForm.last_name &&
        this.userForm.age
      ) {
        axios
          .post(`${API_URL}/users`, this.userForm)
          .then(res => {
            res = this.userForm;
            console.log(res);
            // console.log(this.userForm);
          })
          .catch(error => console.log(error));
        return true;
      }
      this.errors = [];

      if (!this.userForm.username) {
        this.errors.push("Username is required");
      }
      if (!this.userForm.password) {
        this.errors.push("Password is required");
      }
      if (!this.userForm.first_name) {
        this.errors.push("First Name is required");
      }
      if (!this.userForm.last_name) {
        this.errors.push("Last Name is required");
      }
      if (!this.userForm.age) {
        this.errors.push("Age is required");
      }
      e.preventDefault();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
