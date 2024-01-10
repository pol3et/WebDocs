# Register.vue

## Register Component

```html
<template>
    <div class="container">
      <h2>Зарегистрироваться</h2>
      <form @submit.prevent="registerUser">
        <label for="username">Имя пользователя:</label>
        <input type="text" v-model="username" required />
  
        <label for="email">Электронная почта:</label>
        <input type="email" v-model="email" required />
  
        <label for="password">Пароль:</label>
        <input type="password" v-model="password" required />
  
        <button type="submit">Зарегистрироваться</button>
      </form>
    </div>
</template>
```

- **Description**: Vue template for the registration component. Includes a form with input fields for username, email, and password.

## Register Component Script

```html
<script>
import axios from 'axios';
  
export default {
  data() {
    return {
      username: '',
      email: '',
      password: '',
    };
  },
  methods: {
    async registerUser() {
      try {
        const response = await axios.post('http://localhost:8000/auth/users/', {
          username: this.username,
          email: this.email,
          password: this.password,
        });
  
        console.log('User registered successfully:', response.data);
        this.$router.push('/login');
      } catch (error) {
        console.error('Registration failed:', error.response.data);
      }
    },
  },
};
</script>
```

- **Description**: Vue script for the registration component. Defines the `registerUser` method to handle the form submission and make an API request to register a new user.

## Moving on to the [Profile.vue](profile.md)