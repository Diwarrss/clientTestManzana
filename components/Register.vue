<template>
  <b-container class="form-signin">
    <b-row
      align-v="center"
      class="justify-content-center"
      align-h="center"
      style="height: 100vh"
    >
      <b-col md="4">
        <h2 class="text-center">Registro</h2>
        <b-form @submit.stop.prevent="onSubmit">
          <b-form-group id="input-group-1" label="Nombre:" label-for="input-3">
            <b-form-input
              id="input-3"
              v-model="name"
              type="text"
              :class="{ 'is-invalid': submitted && $v.name.$error }"
            ></b-form-input>
            <div v-if="submitted && !$v.name.required" class="invalid-feedback">
              Nombre es requerido.
            </div>
          </b-form-group>
          <b-form-group
            id="input-group-1"
            label="Apellido:"
            label-for="input-5"
          >
            <b-form-input
              id="input-5"
              v-model="last_name"
              type="text"
              :class="{ 'is-invalid': submitted && $v.last_name.$error }"
            ></b-form-input>
            <div
              v-if="submitted && !$v.last_name.required"
              class="invalid-feedback"
            >
              Apellido es requerido.
            </div>
          </b-form-group>
          <b-form-group id="input-group-1" label="E-mail:" label-for="input-1">
            <b-form-input
              id="input-1"
              v-model="email"
              type="email"
              placeholder="E-Mail"
              :class="{ 'is-invalid': submitted && $v.email.$error || errors}"
            ></b-form-input>
            <div v-if="submitted && $v.email.$error" class="invalid-feedback">
              <span v-if="!$v.email.required">Email es requerido.</span>
              <span v-if="!$v.email.email">
                Por favor ingrese un Email válido.
              </span>
            </div>
            <div v-if="errors" class="invalid-feedback">
              <span v-if="errors.email">{{errors.email[0]}}</span>
            </div>
          </b-form-group>
          <b-form-group
            id="input-group-2"
            label="Contraseña:"
            label-for="input-9"
          >
            <b-form-input
              id="input-9"
              v-model="password"
              type="password"
              :class="{ 'is-invalid': submitted && $v.password.$error }"
            ></b-form-input>
            <div
              v-if="submitted && !$v.password.required"
              class="invalid-feedback"
            >
              Contraseña es requerida.
            </div>
          </b-form-group>
          <b-form-group
            id="input-group-2"
            label="Confirmar contraseña:"
            label-for="input-6"
          >
            <b-form-input
              id="input-6"
              v-model="repeatPassword"
              type="password"
              :class="{ 'is-invalid': submitted && $v.repeatPassword.$error }"
            ></b-form-input>
            <div
              v-if="!$v.repeatPassword.required"
              class="invalid-feedback"
            >
              La contraseña de confirmación es requerida.
            </div>
            <div
              v-if="!$v.repeatPassword.sameAsPassword"
              class="invalid-feedback"
            >
              Las contraseñas deben ser idénticas.
            </div>
          </b-form-group>
          <b-row class="justify-content-between my-auto m-0">
            <b-button type="submit" variant="success">Aceptar</b-button>
          </b-row>
        </b-form>
      </b-col>
    </b-row>
  </b-container>
</template>
<script>
import { required, email, minLength, sameAs } from 'vuelidate/lib/validators'
export default {
  data() {
    return {
      submitted: false,
      name: null,
      last_name: null,
      email: null,
      password: null,
      repeatPassword: null,
      errors: null
    }
  },
  validations: {
    name: {
      required,
    },
    last_name: {
      required,
    },
    email: {
      required,
      email,
    },
    password: {
      required,
      minLength: minLength(6),
    },
    repeatPassword: {
      sameAsPassword: sameAs('password'),
    },
  },
  methods: {
    onSubmit() {
      const params = {
        name: this.name,
        last_name: this.last_name,
        email: this.email,
        password: this.password
      }
      try {
        this.submitted = true
        // stop here if form is invalid
        this.$v.$touch()
        if (this.$v.$invalid) {
          return
        } else {
          this.$axios.post('/register', params)
          .then(res => {
            this.$auth.loginWith('jwt', {
              data: {
                email: this.email,
                password: this.password
              }
            })
          })
          .catch(err => {
            if (err) {
              this.errors = err.response.data.errors
            }
          })
        }
      } catch (err) {
        console.log(err)
      }
    },
  },
}
</script>
