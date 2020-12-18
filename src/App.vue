<template>
<div id="app">

    <nav>
      <div class="nav-wrapper blue darken-1">
        <a href="#" class="brand-logo center">Cadastro</a>
      </div>
    </nav>

    <div class="container">

      <ul>
        <li v-for="(erro, index) of errors" :key="index">
          campo <b>{{erro.field}}</b> - {{ erro.defaultMessage }}

        </li>
      </ul>
       
      <form @submit.prevent="salvar">


          <label>ID</label>
          <input type="text" placeholder="ID" v-model="usuario.id">
          <label>Nome</label>
          <input type="text" placeholder="Nome" v-model="usuario.first_name" >
          
          <label>Sobrenome</label>
          <input type="text" placeholder="Sobrenome" v-model="usuario.last_name">
         
          <label>E-mail</label>
          <input type="text" placeholder="E-mail" v-model="usuario.email">

          <button class="waves-effect waves-light btn-small">Salvar<i class="material-icons left">save</i></button>

      </form>

      <table>

        <thead>

          <tr>
            <th>ID</th>
            <th>Nome</th>
            <th>Sobrenome</th>
            <th>Imagem</th>
            <th>Email</th>
            <th>Opções</th>
          </tr>

        </thead>

        <tbody>

          <tr v-for="usuario of usuarios" :key="usuario.id">

            <td>{{usuario.id}}</td>
            <td>{{usuario.first_name}}</td>
            <td>{{usuario.last_name}}</td>
            <td><img :src="usuario.avatar"></td>
            <td>{{usuario.email}}</td>
            <td>
              <button @click="editar(usuario)" class="waves-effect btn-small blue darken-1"><i class="material-icons">create</i></button>
              <button @click="deletar(usuario)" class="waves-effect btn-small red darken-1"><i class="material-icons">delete_sweep</i></button>
            </td>

          </tr>

        </tbody> 
      
      </table>

    </div>

  </div>
  
</template>

<script>
import Usuarios from './services/usuarios'


export default {
 
  data(){

    

    return {

      usuario: {
      id: '',
      first_name: '',
      last_name: '',
      
      
      email: ''

    },

      usuarios: [],
      errors: []
    }

  },

  


  mounted(){
      this.listar()
      
  }, 

  methods: {

    listar(){
      Usuarios.listar().then(response => {
      this.usuarios = response.data.data
    })
  },


    salvar(){
      if(!this.usuario.id){
         Usuarios.salvar(this.usuario).then(() => {
          this.usuario = {}
           alert("Salvo com sucesso")

           this.listar()
           this.errors = []
         
      }).catch(err => {
        
        this.errors = err.response.data.errors
      })
      } else {
         Usuarios.atualizar(this.usuario).then(() => {
          this.usuario = {}
           alert("Atualizado com sucesso")

           this.listar()
           this.errors = []
         
      }).catch(err => {
        
        this.errors = err.response.data.errors
      })
      }



     
    },

  editar(usuario){
      this.usuario = usuario
  },

  deletar(usuario){
    Usuarios.apagar(usuario).then(() => {
        this.listar();
        this.errors = []
    }).catch(err => {
      this.errors = err.response.data.errors
    })
  }



  }


}


</script>

<style>

</style>
