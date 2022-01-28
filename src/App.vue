<template>
       <div id="app" class="container">
              <div class="row">
                     <div class="col-md-12 mt-2">
                            <h1>Usuarios</h1>
                     </div>
              </div>
              <div class="row">
                     <div class="col-md-12">
                            <formulario-usuario @crear-usuario="postUsuario" />
                            <tabla-usuarios
                                   :usuarios="usuarios"
                                   @eliminar-usuario="deleteUsuario"
                                   @actualizar-usuario="putUsuario"
                            />
                     </div>
              </div>
       </div>
</template>

<script>
import TablaUsuarios from "@/components/TablaUsuarios.vue";
import FormularioUsuario from '@/components/FormularioUsuario.vue';

export default {
       name: "App",
       components: {
              TablaUsuarios,
              FormularioUsuario,
       },

       data() {
              return {
                     usuarios: [],
              };
       },

       methods: {
              async getUsuarios() {
                     try {
                            const response = await fetch(
                                   "https://jsonplaceholder.typicode.com/users"
                            );
                            this.usuarios = await response.json();
                     } catch (error) {
                            console.error(error);
                     }
              },
              async postUsuario(usuario) {
                     try {
                            const response = await fetch(
                                   "https://jsonplaceholder.typicode.com/users",
                                   {
                                          method: "POST",
                                          body: JSON.stringify(usuario),
                                          headers: {
                                                 "Content-type":
                                                        "application/json; charset=UTF-8",
                                          },
                                   }
                            );

                            const usuarioCreado = await response.json();
                            this.usuarios = [...this.usuarios, usuarioCreado];
                     } catch (error) {
                            console.error(error);
                     }
              },
              async putUsuario(usuario) {
                     try {
                            const response = await fetch(
                                   `https://jsonplaceholder.typicode.com/users/${usuario.id}`,
                                   {
                                          method: "PUT",
                                          body: JSON.stringify(usuario),
                                          headers: {
                                                 "Content-type":
                                                        "application/json; charset=UTF-8",
                                          },
                                   }
                            );

                            const usuarioActualizado = await response.json();
                            this.usuarios = this.usuarios.map((u) =>
                                   u.id === usuario.id ? usuarioActualizado : u
                            );
                     } catch (error) {
                            console.error(error);
                     }
              },
              async deleteUsuario(usuario) {
                     try {
                            await fetch(
                                   `https://jsonplaceholder.typicode.com/usuarios/${usuario.id}`,
                                   {
                                          method: "DELETE",
                                   }
                            );

                            this.usuarios = this.usuarios.filter(
                                   (u) => u.id !== usuario.id
                            );
                     } catch (error) {
                            console.error(error);
                     }
              },
       },

       mounted() {
              this.getUsuarios();
       },
};
</script>
