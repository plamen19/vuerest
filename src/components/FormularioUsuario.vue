<template>
       <div id="formulario-usuario">
              <form @submit.prevent="enviarFormulario">
                     <div class="container">
                            <div class="row">
                                   <div class="col-md-4">
                                          <div class="form-group">
                                                 <label>Nombre</label>
                                                 <input
                                                        ref="name"
                                                        v-model="usuario.name"
                                                        type="text"
                                                        class="form-control"
                                                        :class="{
                                                               'is-invalid':
                                                                      procesando &&
                                                                      nameInvalido,
                                                        }"
                                                        @focus="resetEstado"
                                                        @keypress="resetEstado"
                                                 />
                                          </div>
                                   </div>
                                   <div class="col-md-4">
                                          <div class="form-group">
                                                 <label>Email</label>
                                                 <input
                                                        v-model="usuario.email"
                                                        type="email"
                                                        :class="{
                                                               'is-invalid':
                                                                      procesando &&
                                                                      emailInvalido,
                                                        }"
                                                        class="form-control"
                                                        @focus="resetEstado"
                                                 />
                                          </div>
                                   </div>
                            </div>
                            <div class="row">
                                   <div class="col-md-4">
                                          <div class="form-group">
                                                 <button
                                                        class="btn btn-primary"
                                                 >
                                                        Añadir usuario
                                                 </button>
                                          </div>
                                   </div>
                            </div>
                     </div>
                     <div class="container">
                            <div class="row">
                                   <div class="col-md-12">
                                          <div
                                                 v-if="error && procesando"
                                                 class="alert alert-danger"
                                                 role="alert"
                                          >
                                                 Debes rellenar todos los
                                                 campos!
                                          </div>
                                          <div
                                                 v-if="correcto"
                                                 class="alert alert-success"
                                                 role="alert"
                                          >
                                                 El usuario ha sido agregado
                                                 correctamente!
                                          </div>
                                   </div>
                            </div>
                     </div>
              </form>
       </div>
</template>

<script>
export default {
       name: "formulario-usuario",
       data() {
              return {
                     procesando: false,
                     correcto: false,
                     error: false,
                     usuario: {
                            name: "",
                            email: "",
                     },
              };
       },
       methods: {
              enviarFormulario() {
                     this.procesando = true;
                     this.resetEstado();

                     // Comprobamos la presencia de errores
                     if (this.nameInvalido || this.emailInvalido) {
                            this.error = true;
                            return;
                     }

                     this.$emit("crear-usuario", this.usuario);
                     this.$refs.name.focus();
                     this.error = false;
                     this.correcto = true;
                     this.procesando = false;

                     // Restablecemos el valor de la variables
                     this.usuario = {
                            name: "",
                            email: "",
                     };
              },
              resetEstado() {
                     this.correcto = false;
                     this.error = false;
              },
       },
       computed: {
              nameInvalido() {
                     return this.usuario.name.length < 1;
              },
              emailInvalido() {
                     return this.usuario.email.length < 1;
              },
       },
};
</script>
