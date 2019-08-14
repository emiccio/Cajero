<template>
  <div>
    <div v-if="auth === false">
      <login @enter="validar"/>
    </div>
    <div v-else>
      <cajero :usuario='usuarioLogueado'/>
    </div>
  </div>
</template>

<script>  
import Cajero from './components/Cajero.vue'
import Login from './components/Login.vue'

export default {
  name: 'app',
  components: {
    Cajero,
    Login
  },
  data() {
    return {
      auth: false,
      usuarios: [],
      usuarioLogueado: {}
    }
  },
  mounted(){
    //llamar a metodo cargarUsuario
    this.cargarUsuario()
  },
  methods: {
    cargarUsuario: async function(){
      const data = await fetch('usuarios.json')
      this.usuarios = await data.json()
    },
    validar(pass) {
      for(let i=0; (i<this.usuarios.length && this.auth) == false; i++){
        //console.log('hola usuario',i)
        //console.log('hola', this.usuarios[i].nombre + 'tu saldo es $',this.usuarios[i].saldo)
        //console.log(this.usuarios[i].contraseña)
        if(pass == this.usuarios[i].contraseña){
          //console.log('hola', this.usuarios[i].nombre + ' tu saldo es $',this.usuarios[i].saldo)
          this.usuarioLogueado = this.usuarios[i]
          this.auth = true
        }
      }
    }
  }
}
</script>


<style>
@import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');
  body {
    margin: 0;

    font-family: "Source Sans Pro", sans-serif;

    background-color: #2a333d;
}
</style>
