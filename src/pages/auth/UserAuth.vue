<template>
  <div>
    <base-dialogue
      :show="!!error"
      title="An error occured"
      @close="error = null"
    >
      <p>{{ error }}</p>
    </base-dialogue>
    <base-dialogue :show="isLoading" fixed title="Authenticating...">
      <base-spinner></base-spinner>
    </base-dialogue>
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
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      formIsValid: true,
      mode: 'login',
      isLoading: false,
      error: null,
    };
  },
  methods: {
    async submitForm() {
      this.formIsValid = true;
      if (this.email === '' || this.password.length < 6) {
        this.formIsValid = false;
        return;
      }

      this.isLoading = true;
      // send http request
      try {
        if (this.mode === 'login') {
          // login
          await this.$store.dispatch('login', {
            email: this.email,
            password: this.password,
          });
        } else {
          await this.$store.dispatch('signup', {
            email: this.email,
            password: this.password,
          });
        }
      } catch (error) {
        this.error = error.message || 'Something went wrong';
      }
      this.isLoading = false;
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
