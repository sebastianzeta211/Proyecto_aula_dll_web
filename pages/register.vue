<template>
    <v-parallax src="https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg">
      <div class="d-flex flex-column fill-height justify-center align-center text-white">
        <h1 class="text-h4 font-weight-thin mb-4">
          Registro de Usuarios
        </h1>
        <h4 class="subheading"></h4>
        <div id="1" class="pa-4 primary white--text rounded-pill" style="weight: 100vh">
          <div class="flex-column justify-center align-center text-white" style="weight: 100vh">
          
              <v-form fast-fail  style="weight: 100vh">
                <v-text-field v-model="username" label="Nombre completo"></v-text-field>
                <v-text-field v-model="username" label="Correo"></v-text-field>
  
                <v-text-field v-model="password" label="password"></v-text-field>
               
                <v-btn type="submit" color="primary" block class="mt-2">Registrar</v-btn>
  
              </v-form>
              
              <v-btn type="button" color="secundary" block class="mt-2">regresar</v-btn>
  
          </div>
        </div>
      </div>
    </v-parallax>
  </template>
    <script>
    import axios from 'axios';
    
    export default {
      data() {
        return {
          email: '',
          password: '',
          visible: false,
          users: [],
          emailRules: [
            value => {
              if (value) return true
              return 'El campo es obligatorio.'
            },
            value => {
              if (/[^[^@]+@[^@]+\.[a-zA-Z]{2,}$/.test(value)) return true
              return 'Correo no valido.'
            }
          ],
        };
      },
      methods: {
        async getUsers() {
          try {
            const response = await axios.get('http://localhost:3001/usersvalidate');
            this.users = response.data;
          } catch (error) {
            console.error('Error al obtener usuarios:', error);
          }
        },
        async login() {
          await this.getUsers();
    
          const foundUser = this.users.find(
            user =>
              user.email === this.email && user.password === this.password
          );
    
          if (foundUser) {
            console.log('Inicio de sesión exitoso para el usuario:', foundUser);
            this.$router.push('/index');
          } else {
            console.error('Credenciales incorrectas. Inicio de sesión fallido.');
          }
        },
      },
    };
    definePageMeta({
      layout: "blank",
    });
      </script>