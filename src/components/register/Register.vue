<template>
  <div>
      <h2>Register New User</h2>
      <form @submit.prevent="checkForm">
          <div v-if="errors.length">
            <p><b>Please correct the following errors:</b></p>
            <ul>
                <li v-for="(error, i) in errors" :key="i">
                    <p>{{ error }}</p>
                </li>
            </ul>
          </div>

          <div>
              <div>
                  <label for="firstname">First name</label>
                  <input v-model="user.firstname" type="text" id="firstname" name="firstname">
              </div>
              <div>
                  <label for="lastname">Lastname</label>
                  <input v-model="user.lastname" type="text" id="lastname" name="lastname">
              </div>
              <div>
                  <label for="username">Username</label>
                  <input v-model="user.username" type="text" id="username" name="username">
              </div>
              <div>
                  <label for="email">Email</label>
                  <input v-model="user.email" type="email" id="email" name="email">
              </div>
              <div>
                  <input type="submit" value="Register">
              </div>
          </div>
      </form>
  </div>
</template>

<script>
export default {
    name: "Register",

    data: function() {
        return {
            errors: [],
            user: {
                firstname: "",
                lastname: "",
                username: "",
                email: ""
            }
        }
    },

    methods: {
        checkForm: function(event) {
            event.preventDefault();
            this.errors = [];
            if (!this.user.firstname) {
                this.errors.push("Firstname Required");
            }
            if (!this.user.lastname) {
                this.errors.push("Lastname Required");
            }
            if (!this.user.username) {
                this.errors.push("Username Required");
            }
            if (!this.user.email) {
                this.errors.push("Email Required");
            }
            if (!this.errors.length) {
                this.registerUser(this.user);
            }
        },
        registerUser: function(user) {
            this.$http.post(`${process.env.VUE_APP_API_URL}users/register`, user)
            .then((response) => {
            if (response.body) {
              localStorage.loggedIn = true;
              localStorage.user = user.email;
              this.$emit("$loggedIn", true);
              this.$router.push({ path: "/" });
            }
          },
          (response) => {
            this.errors.push(response.body.message);
          }
        )
        }
    }
};
</script>

<style>

</style>