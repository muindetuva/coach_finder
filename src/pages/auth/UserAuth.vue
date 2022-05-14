<template>
  <base-card>
    <form @submit.prevent="submitForm">
      <div class="form-control">
        <label for="email">email</label>
        <input type="email" name="email" id="email" v-model.trim="email" />
      </div>
      <div class="form-control">
        <label for="password">Password</label>
        <input
          type="password"
          name="password"
          id="password"
          v-model.trim="password"
        />
      </div>
      <p v-if="!formIsValid">Please enter a valid email and password</p>

      <base-button>{{ submitButtonCaption }}</base-button>
      <base-button type="button" mode="flat" @click="switchAuthMode">{{
        submitModeButtonCaption
      }}</base-button>
    </form>
  </base-card>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      formIsValid: true,
      mode: 'login',
    };
  },
  methods: {
    submitForm() {
      this.formIsValid = true;
      if (this.email === '' || this.password.length < 6) {
        this.formIsValid = false;
        return;
      }

      // send http request
      if (this.mode === 'login') {
        // login
      } else {
        this.$store.dispatch('signup', {
          email: this.email,
          password: this.password,
        });
      }
    },
    switchAuthMode() {
      this.mode = this.mode === 'login' ? 'signup' : 'login';
    },
  },
  computed: {
    submitButtonCaption() {
      return this.mode === 'login' ? 'Login' : 'Signup';
    },
    submitModeButtonCaption() {
      return this.mode === 'login' ? 'Signup Instead' : 'Login Instead';
    },
  },
};
</script>

<style scoped>
form {
  margin: 1rem;
  border-radius: 12px;
  padding: 1rem;
}

.form-control {
  margin: 0.5rem 0;
}

label {
  font-weight: bold;
  margin-bottom: 0.5rem;
  display: block;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  border: 1px solid #ccc;
  padding: 0.15rem;
}

input:focus,
textarea:focus {
  border-color: #3d008d;
  background-color: #faf6ff;
  outline: none;
}
</style>
