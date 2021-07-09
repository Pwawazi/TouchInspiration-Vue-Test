<template>
  <div class="submit-form">
    <div v-if="!submitted">
      <div class="form-group">
        <label for="name">Name</label>
        <input
          type="text"
          class="form-control"
          id="name"
          required
          v-model="user.name"
          name="name"
        />
      </div>

      <div class="form-group">
        <label for="occupation">Occupation</label>
        <input
          type="text"
          class="form-control"
          id="occupation"
          required
          v-model="user.occupation"
          name="occupation"
        />
      </div>

      <div class="form-group">
        <label for="email">Email</label>
        <input
          type="text"
          class="form-control"
          id="email"
          required
          v-model="user.email"
          name="email"
        />
      </div>

      <div class="form-group">
        <label for="bio">Bio</label>
        <input
          class="form-control"
          id="bio"
          required
          v-model="user.bio"
          name="bio"
        />
      </div>

      <button @click="saveUser" class="btn btn-success my-4">Submit</button>
    </div>

    <div v-else>
      <h4>You submitted successfully!</h4>
      <router-link to="/" class="nav-link my-4">Back To Users</router-link>
    </div>
  </div>
</template>

<script>
import UserDataService from "../services/UserDataService";

export default {
  name: "add-user",
  data() {
    return {
      user: {
        id: null,
        name: "",
        occupation: "",
        email: "",
        bio: "",
      },
      submitted: false
    };
  },
  methods: {
    saveUser() {
      var data = {
        name: this.user.name,
        occupation: this.user.occupation,
        email: this.user.email,
        bio: this.user.bio
      };

      UserDataService.create(data)
        .then(response => {
          this.user.id = response.data.id;
          console.log(response.data);
          this.submitted = true;
        })
        .catch(e => {
          console.log(e);
        });
    },
    
    newUser() {
      this.submitted = false;
      this.user = {};
    }
  }
};
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>