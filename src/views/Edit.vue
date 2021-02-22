<template>
  <div>
      <h2>Edicao de Usuario</h2>
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
            <hr>
            <button class="button is-success" @click="update()">Editar</button>
        </div>
      </div>      
  </div>
</template>

<script>
import axios from 'axios'//Importando o axios para que seja utilizado nas resquisicoes

export default {
    created(){
        let req = {//Cabecalho contendo dados para autenticacao que gera acesso a rota solicitada
          headers: {
            Authorization: "Bearer " + localStorage.getItem('token')
      }
    } 

        axios.get('http://localhost:8686/user/' + this.$route.params.id, req).then(res => {
            console.log(res)

            this.name = res.data.name
            this.email = res.data.email
            this.id = res.data.id

        }).catch(error => {
            console.log(error.response)
            this.$router.push({name: 'Users'})
        })
    },
    data(){
        return {
            name:'',            
            email:'',
            id: -1,
            error: undefined
        }
    },
    methods: {
        update(){

            let req = {//Cabecalho contendo dados para autenticacao que gera acesso a rota solicitada
                headers: {
                    Authorization: "Bearer " + localStorage.getItem('token')
                }
            } 
            
            axios.put('http://localhost:8686/user/update',{ //Insiro no objeto os dados necessario para cadastro na API
                id: this.id,
                name: this.name,
                email: this.email                
            }, req).then(res => {
                console.log(res.data.message)
                this.$router.push({name: 'Users'})//Utilizando router do Vue para direcionar em caso de sucesso para pagina Home
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