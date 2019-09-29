<template>
  <div class="home">
    <table class="table">
      <thead>
      <tr>
        <th>Name</th>
        <th>Surname</th>
        <th>Email</th>
        <th colspan="3">Phone</th>
      </tr>
      </thead>
      <tbody v-if="users && users.length">
      <tr v-for="user in users" :key="user.id">
        <td>{{user.name}}</td>
        <td>{{user.surname}}</td>
        <td>{{user.email}}</td>
        <td>{{user.phone}}</td>
        <td class="table-button-wrapper">
          <router-link
            :to="{name: 'edit', params: {editUser: user}}"
            class="table__button"
            tag="button">
            <i class="fas fa-edit"></i>
          </router-link>
        </td>
        <td class="table-button-wrapper">
          <button class="table__button" @click="deleteUser(user.id)">
            <i class="fas fa-trash-alt"></i>
          </button>
        </td>
      </tr>
      </tbody>
      <tbody v-else>
      <tr>
        <td colspan="4" :style="{textAlign: 'center'}">Users not found</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex';

export default {
  name: 'home',

  computed: mapState(['users']),

  created() {
    this.getUsers();
  },

  methods: {
    ...mapActions(['getUsers', 'setUsers']),

    deleteUser(id) {
      this.setUsers(this.users.filter(user => user.id !== id));
    },
  },
};
</script>

<style lang="scss">
.home{
  width: 1024px;
  margin: 0 auto;
  .table{
    width: 100%;
    border-collapse: collapse;
    th, td {
      padding: 8px;
      text-align: center;
    }
    tr:nth-child(even){
      background-color: #f2f2f2
    }
    th {
      background-color: #bdbdbd;
      color: white;
    }
    &-button-wrapper{
      width: 15px;
    }
    &__button {
      background: inherit;
      border: none;
      cursor: pointer;
    }
  }
}
</style>
