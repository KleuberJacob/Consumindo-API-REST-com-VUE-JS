<template>
  <div>
      <h2>Registro de Usuario</h2>
      <hr>
      <div v-if="error != undefined">
          <div class="notification is-danger">
              {{error}}
          </div>          
      </div>
      <div class="columns is-centered">
        <div class="column is-half">
            <p>Nome</p>
            <input type="text" placeholder="Nome do Usuario" class="input" v-model="name">
            <p>E-mail</p>
            <input type="email" placeholder="email@email.com" class="input" v-model="email">
            <p>Senha</p>
            <input type="password" placeholder="********" class="input" v-model="password">
            <hr>
            <button class="button is-success" @click="register()">Cadastrar</button>
        </div>
      </div>      
  </div>
</template>

<script>
import axios from 'axios'//Importando o axios para que seja utilizado nas resquisicoes

export default {
    data(){
        return {
            name:'',            
            email:'',
            password:'',
            error: undefined
        }
    },
    methods: {
        register(){
            axios.post('http://localhost:8686/user/create',{ //Insiro no objeto os dados necessario para cadastro na API
                name: this.name,
                email: this.email,
                password: this.password
            }).then(res => {
                console.log(res.data.message)
                this.$router.push({name: 'Home'})//Utilizando router do Vue para direcionar em caso de sucesso para pagina Home
            }).catch(err => {
                let msgError = err.response.data.error
                this.error = msgError
            })
        }
    }
    
}
</script>

<style>

</style>