<template>
  <div class="wrapper">
    <div class="tab">
      <button
        @click="statusForm = 'edit'"
        :class="['tab__item', {'tab__item-active': statusForm === 'edit'}]"
      >Edit user</button>
      <button
        @click="statusForm = 'json'"
        :class="['tab__item', {'tab__item-active': statusForm === 'json'}]"
      >Add json</button>
    </div>

    <form
      v-if="statusForm === 'edit'"
      class="form"
      @submit.prevent="handleEditSubmit" >
      <input
        type="text"
        v-model.trim="$v.user.name.$model"
        :class="['form-input', {'form-input__error': $v.user.name.$error}]"
        @blur="$v.user.name.$touch()"
        placeholder="Name"
      >
      <input
        type="text"
        v-model.trim="$v.user.surname.$model"
        :class="['form-input', {'form-input__error': $v.user.surname.$error}]"
        @blur="$v.user.surname.$touch()"
        placeholder="Surname"
      >
      <input
        type="text"
        v-model.trim="$v.user.email.$model"
        :class="['form-input', {'form-input__error': $v.user.email.$error}]"
        @blur="$v.user.email.$touch()"
        placeholder="Email"
      >
      <input
        type="text"
        v-model.trim="$v.user.phone.$model"
        :class="['form-input', {'form-input__error': $v.user.phone.$error}]"
        @blur="$v.user.phone.$touch()"
        placeholder="Phone"
      >
      <button :class="['form-button', {'form-button__disabled': $v.user.$invalid}]" >
        Save
      </button>
    </form>

    <form
      v-else
      class="form"
      @submit.prevent="handleJsonSubmit"
    >
      <textarea
        cols="30"
        rows="10"
        v-model.trim="$v.jsonUser.$model"
        :class="['form-input', {'form-input__error': $v.jsonUser.$error}]"
        placeholder="Add json"
        @blur="$v.jsonUser.$touch()"
      ></textarea>
      <button :class="['form-button', {'form-button__disabled': $v.jsonUser.$invalid}]" >
        Save
      </button>
    </form>
  </div>
</template>

<script>
import { required, email, minLength } from 'vuelidate/lib/validators';
import { mapState, mapActions } from 'vuex';
import uuidv4 from 'uuid/v4';

const validateNumber = value => /^[\\\\+]?[(]?[0-9]{3}[)]?[-\\\\s\\\\.]?[0-9]{3}[-\\\\s\\\\.]?[0-9]{4,7}$/im.test(value);
const validateJSON = (value) => {
  try {
    JSON.parse(value);
  } catch (e) {
    return false;
  }
  return true;
};
export default {
  props: {
    editUser: {
      type: Object,
      default() {
        return {
          name: '',
          surname: '',
          email: '',
          phone: '',
        };
      },
    },
  },

  validations: {
    user: {
      name: {
        required,
        minLength: minLength(3),
      },
      surname: {
        required,
        minLength: minLength(3),
      },
      email: {
        required,
        email,
      },
      phone: {
        required,
        validateNumber,
      },
    },
    jsonUser: {
      required,
      validateJSON,
    },
  },

  data() {
    return {
      user: {
        name: this.editUser.name,
        surname: this.editUser.surname,
        email: this.editUser.email,
        phone: this.editUser.phone,
      },
      statusForm: 'edit',
      jsonUser: '',
    };
  },

  computed: mapState(['users']),

  created() {
    this.getUsers();
  },

  methods: {
    ...mapActions(['getUsers', 'setUsers']),

    handleEditSubmit() {
      if (!this.$v.user.$invalid) {
        let updateUser;
        if (this.editUser.id) {
          updateUser = this.users.map((user) => {
            if (user.id === this.editUser.id) {
              return {
                ...user,
                name: this.user.name,
                surname: this.user.surname,
                phone: this.user.phone,
                email: this.user.email,
              };
            }
            return user;
          });
        } else if (this.users) {
          updateUser = [...this.users, {
            id: uuidv4(),
            name: this.user.name,
            surname: this.user.surname,
            phone: this.user.phone,
            email: this.user.email,
          }];
        } else {
          updateUser = [{
            id: uuidv4(),
            name: this.user.name,
            surname: this.user.surname,
            phone: this.user.phone,
            email: this.user.email,
          }];
        }

        this.setUsers(updateUser);
        this.$router.push({ name: 'home' });
      }
    },

    handleJsonSubmit() {
      if (!this.$v.jsonUser.$invalid) {
        let users = JSON.parse(this.jsonUser);
        users = users.map(user => ({
          ...user,
          id: uuidv4(),
        }));
        this.setUsers([...this.users, ...users]);
        this.$router.push({ name: 'home' });
      }
    },
  },
};
</script>

<style scoped lang="scss">
.wrapper {
  width: 1024px;
  margin: 0 auto;

  .tab {
    margin: 0 auto;
    width: 320px;
    &__item {
      width: 50%;
      padding: 10px 0;
      border: none;
      &-active {
        border: 1px solid #1b5e20;
      }
    }
  }

  .form {
    width: 320px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    &-input {
      margin: 7px 0;
      padding: 8px 5px;
      font-size: 15px;
      border: 1px solid #1b5e20;
      border-radius: 3px;

      &__error {
        border: 1px solid #f44336;
      }
    }
    &-button {
      background: #1b5e20;
      color: #ffffff;
      padding: 10px 0;
      font-size: 16px;
      border: none;
      border-radius: 3px;
      cursor: pointer;

      &__disabled {
        background: #bdbdbd;
        cursor: not-allowed;
      }
    }
  }
}

</style>
