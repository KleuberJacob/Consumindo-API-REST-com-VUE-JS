<template>
  <div>
      <h2>Login de Usuarios</h2>
      <hr>
      <div v-if="error != undefined">
          <div class="notification is-danger">
              {{error}}
          </div>          
      </div>
      <div class="columns is-centered">
        <div class="column is-half">            
            <p>E-mail</p>
            <input type="email" placeholder="email@email.com" class="input" v-model="email">
            <p>Senha</p>
            <input type="password" placeholder="********" class="input" v-model="password">
            <hr>
            <button class="button is-success" @click="login()">Acessar</button>
        </div>
      </div>      
  </div>
</template>

<script>
import axios from 'axios'//Importando o axios para que seja utilizado nas resquisicoes

export default {
    data(){
        return {                        
            email:'',
            password:'',
            error: undefined
        }
    },
    methods: {
        login(){
            axios.post('http://localhost:8686/login',{ //Insiro no objeto os dados necessario para cadastro na API
                email: this.email,
                password: this.password
            }).then(res => {
                localStorage.setItem('token', res.data.token)//Armazenando token de autenticacao no localStorage
                this.$router.push({name: 'Home'})                                
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