<template>
  <q-page class="flex flex-center">
    <q-form @submit="handleLogin" style="width: 300px;">
      <q-input v-model="email" label="Email" outlined />
      <q-input v-model="password" label="Password" type="password" outlined />
      <q-btn type="submit" label="Login" color="primary" class="full-width q-mt-md" />
    </q-form>
  </q-page>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      email: '',
      password: '',
    };
  },
    methods: {
    async handleLogin() {
      try {
        const response = await axios.post(
          'http://68.183.142.21/api/v1/user/signin',
          { email: this.email, password: this.password }
        );

        if (response.status === 200) {

          localStorage.setItem('authToken', response.data.token);


          // Redirecciona a la página de películas si el login es exitoso
          this.$router.push('/movies');
        }
      } catch (error) {
        // Muestra un mensaje de error si las credenciales son incorrectas
        this.$q.notify({
          type: 'negative',
          message: 'Credenciales incorrectas. Por favor, inténtalo de nuevo.',
          position: 'top',
        });
      }
    },
  },
};
</script>
