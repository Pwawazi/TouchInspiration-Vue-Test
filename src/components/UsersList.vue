<template>
  <div class="list row">
    <div class="col-md-6">
      <h4>Users List</h4>
          <table class="table table-bordered table-hover">
          <thead class="thead-light">
            <tr>
              <th scope="col">Name</th>
              <th scope="col">Occupation</th>
              <th scope="col">Email</th>
            </tr>
          </thead>
          <tbody 
          
          :class="{ active: index == currentIndex }"
          v-for="(user, index) in users"
          :key="index"
          @click="setActiveUser(user, index)"
          >
            <tr>
              <td class="point">{{ user.name }}</td>
              <td class="point">{{ user.occupation }}</td>
              <td class="point">{{ user.email }}</td>
            </tr>
          </tbody>
          </table>
      <button class="m-3 btn btn-sm btn-danger" @click="removeAllUsers">
        Remove All
      </button>
    </div>
    <div class="col-md-6">
      <div v-if="currentUser" class="m-5" id="fixed">
        <h4>User</h4>
        <div class="py-2">
          <label><strong>Name:</strong></label> {{ currentUser.name }}
        </div>
        <div class="py-2">
          <label><strong>Bio:</strong></label> {{ currentUser.bio }}
        </div>

        <router-link :to="'/users/' + currentUser.id" class="btn btn-warning">Edit</router-link>
      </div>
      <div v-else class="m-3" id="fixed">
        <br />
        <p class="m-3">Click on a user to view</p>
      </div>
    </div>
  </div>
</template>

<script>
import UserDataService from "../services/UserDataService";

export default {
  name: "users-list",
  data() {
    return {
      users: [],
      currentUser: null,
      currentIndex: -1,
      name: ""
    };
  },
  methods: {
    retrieveUsers() {
      UserDataService.getAll()
        .then(response => {
          this.users = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    refreshList() {
      this.retrieveUsers();
      this.currentUser = null;
      this.currentIndex = -1;
    },

    setActiveUser(user, index) {
      this.currentUser = user;
      this.currentIndex = user ? index : -1;
    },

    removeAllUsers() {
      UserDataService.deleteAll()
        .then(response => {
          console.log(response.data);
          this.refreshList();
        })
        .catch(e => {
          console.log(e);
        });
    },
    
    searchName() {
     UserDataService.findByName(this.name)
        .then(response => {
          this.users = response.data;
          this.setActiveUser(null);
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.retrieveUsers();
  }
};
</script>

<style>
.list {
  text-align: left;
  margin: auto;
}
.point {
  cursor: pointer;
}
#fixed {
  position: fixed;
}
</style>