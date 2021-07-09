<template>
  <div v-if="currentUser" class="edit-form">
    <h4>User</h4>
    <form class="form">
      <div class="m-3 form-group">
        <label for="name">Name</label>
        <input type="text" class="form-control" id="name"
          v-model="currentUser.name"
        />
      </div>
      <div class="m-3 form-group">
        <label for="email">Email</label>
        <input type="text" class="form-control" id="email"
          v-model="currentUser.email"
        />
      </div>

      <div class="m-3 form-group">
        <label for="description">Bio</label>
        <textarea rows="4" cols="50" class="form-control" id="bio"
          v-model="currentUser.bio"
        />
      </div>
    </form>

    <button class="m-3 btn btn-danger"
      @click="deleteUser"
    >
      Delete
    </button>

    <button type="submit" class="m-3 btn btn-success"
      @click="updateUser"
    >
      Update
    </button>
    <p>{{ message }}</p>
    <router-link to="/" class="nav-link">Back to Users</router-link>
  </div>

  <div v-else>
    <br />
    <p>Please click on a User...</p>
  </div>
</template>

<script>
import UserDataService from "../services/UserDataService";

export default {
  name: "user",
  data() {
    return {
      currentUser: null,
      message: ''
    };
  },
  methods: {
    getUser(id) {
      UserDataService.get(id)
        .then(response => {
          this.currentUser = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    updatePublished(status) {
      var data = {
        id: this.currentUser.id,
        name: this.currentUser.name,
        description: this.currentUser.description,
        published: status
      };

      UserDataService.update(this.currentUser.id, data)
        .then(response => {
          console.log(response.data);
          this.currentUser.published = status;
          this.message = 'The User was updated successfully!';
        })
        .catch(e => {
          console.log(e);
        });
    },

    updateUser() {
      UserDataService.update(this.currentUser.id, this.currentUser)
        .then(response => {
          console.log(response.data);
          this.message = 'The User was updated successfully!';
        })
        .catch(e => {
          console.log(e);
        });
    },

    deleteUser() {
      UserDataService.delete(this.currentUser.id)
        .then(response => {
          console.log(response.data);
          this.$router.push({ name: "users" });
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.message = '';
    this.getUser(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 750px;
  margin: auto;
}
.form {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
}
</style>