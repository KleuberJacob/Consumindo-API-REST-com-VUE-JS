<template>
  <div>
    <h1>Painel Adm</h1>
      
    <table class="table">
      <thead>
        <tr>
          <th>Nome</th>
          <th>E-mail</th>
          <th>Cargo</th>            
          <th>Acoes</th>            
        </tr>
      </thead>        
      <tbody>
          <tr v-for="user in users" :key="user.id">
              <td>{{ user.name }}</td>
              <td>{{ user.email }}</td>
              <td>{{ user.role | processRole }}</td>
              <td>
                  <router-link :to="{ name: 'UserEdit', params:{id: user.id}}"><button class="button is-success">Editar</button></router-link>
                  <button class="button is-danger" @click="showModalUser(user.id)">Excluir</button>
              </td>
          </tr>
      </tbody>
    </table>

    <div :class="{modal: true, 'is-active': showModal}">
      <div class="modal-background"></div>
        <div class="modal-content">

          <div class="card">
            <header class="card-header">
              <p class="card-header-title">
                Deseja deletar esse usuario?
              </p>    
            </header>
            <div class="card-content">
              <div class="content">
                Teste              
              </div>
            </div>
            <footer class="card-footer">
              <a href="#" class="card-footer-item" @click="hideModal()">Cancelar</a>
              <a href="#" class="card-footer-item" @click="deleteUser()">Sim, desejo excluir!</a>              
            </footer>
          </div>
        </div>
      <button class="modal-close is-large" aria-label="close" @click="hideModal()"></button>
    </div>
  </div>  
</template>

<script>
import axios from 'axios'

export default {
    created(){
      let req = {
        headers: {
          Authorization: "Bearer " + localStorage.getItem('token')
      }
    }   

      axios.get('http://localhost:8686/users',req).then(res => {
        console.log(res)
        this.users = res.data        
      }).catch(error => {
        console.log(error)
      })      
    },
    data(){
      return {
        users: [],
        showModal: false,
        deleteUserId: -1 //Utilizamos -1 como padrao devido nao possuirmos nenhum user salvo no DB com essa ID
      }
    },
    methods: {
      hideModal(){
        this.showModal = false
      },
      showModalUser(id){
        this.deleteUserId = id
        this.showModal = true
      }, 
      deleteUser() {
        let req = {//Cabecalho contendo dados para autenticacao que gera acesso a rota solicitada
          headers: {
            Authorization: "Bearer " + localStorage.getItem('token')
      }
    } 

        axios.delete('http://localhost:8686/user/delete/' + this.deleteUserId, req).then(res => {
          console.log(res)
          this.showModal = false //Apos realizada acao fechamos o modal
          this.users = this.users.filter(user => user.id != this.deleteUserId)
        }).catch(error => {
            console.log(error)
            this.showModalUser = false
        })
      }
    },
    filters: {
      processRole: function(value){
        if(value == 0){
            return 'Usuario Comum'
        }else if(value == 1){
            return 'Administrador'
        }
      }
    }
}
</script>

<style>

</style>