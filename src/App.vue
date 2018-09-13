<template>
  <div id="app">
    <div class="buscador-div">
      
      <img  class="dist-logo" src="./assets/logo.png" fluid alt="Responsive image">
      <input class="buscador" type="text" name="buscador">
      <img class="buscador-logo" src="./assets/search.png" v-on:click="buscar">
      <a href="">Registrarme</a>
      <a href="">Iniciar Sesión</a>
      <img style="padding-left:5px; " class="dist-logo" src="./assets/chile.png" fluid alt="Responsive image">
    </div>
    <div class="contenido-div">

   
    <div class="horizontal"  v-for="user in users" style="border: solid 1px black;margin: 5px">
      <div>
      <a href="">{{user.product_name}}</a>
      </div>
      <div>
      {{user.description}}
      </div>
      <div>
      <a style="color:blue;">{{user.price}}</a>
      </div>

    </div>
    <div class="insertar">
      <input type="text" v-model="insertar_name">
      <input type="text" v-model="insertar_description">
      <input type="text" v-model="insertar_price">
      <button v-on:click="send">Publicar</button>
    </div>


    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data(){
    return{
      users:[],
      usuario:{
        product_name:"prueba2",
        description:"descripcion de prueba",
        price:"$1.000.000"
      },
      insertar_price:"",
      insertar_description:"",
      insertar_name:""

    }
  },
  methods:{
    buscar:function(){
      
      
      this.$http.get('http://localhost:3000/users',{headers: {'Content-Type': 'application/json',"Access-Control-Allow-Origin":"*"}})
      .then(response=>{
         // get body data
        this.users = response.body;
       console.log('users',this.users)

      }, response=>{
         // error callback
         console.log('error cargando lista');
      })

    },
    send:function(){
      let usuario=this.usuario
      usuario.product_name=this.insertar_name
      usuario.description=this.insertar_description
      usuario.price="$"+this.insertar_price
      console.log(usuario)
      this.$http.post("http://localhost:3000/sendUser",usuario,{headers:{'Content-Type':'application/json',"Access-Control-Allow-Origin":"*"}}).
        then(response=>{
          this.users=response.body
          console.log("users",this.users)
          this.insertar_name=""
          this.insertar_description=""
          this.insertar_price=""
          this.buscar()
        }, response=>{
         // error callback
         console.log('error cargando lista');
         })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
  padding-top: 12px;
  
}
html{
  background-color: #eee;
}
a{
  margin-left: 1em;
}

.vertical{

  display: flex;
  flex-direction: column;

}
.horizontal{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  height: 1em;
  width: 98%;
}
.buscador{
  width: 65%;
  margin-left: 1em;
}
.buscador-div{
  display: flex;
  flex-direction: row;
  height: 3em;
  border-bottom: ridge;
}
.buscador-logo{
  background-color: green;
  height: 100%;
}
.dist-logo{
  height: 110%;

}
.contenido-div{
  text-align: left;
  height: 30em;
  width: 80%;
  border: solid;
}
.insertar{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
</style>
