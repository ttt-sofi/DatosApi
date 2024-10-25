<script>
import axios from "axios";
import Usuario from "./components/Usuario.vue"; // Importamos el componente Usuario

export default {
  components: {
    Usuario,
  },
  data() {
    return {
      user1: null, // Datos del primer usuario
      user2: null, // Datos del segundo usuario
      mensajes: [], // Lista de mensajes con texto y usuario
    };
  },
  methods: {
    async cargarUsuarios() {
      try {
        const url = "https://randomuser.me/api/?results=2"; // Obtener dos usuarios aleatorios
        const { data } = await axios.get(url);
        this.user1 = data.results[0]; // Primer usuario
        this.user2 = data.results[1]; // Segundo usuario
      } catch (error) {
        console.log(error);
      }
    },

    agregarMensaje({ texto, id, color }) {
      this.mensajes.push({
        texto: texto, // Texto del mensaje
        id: id, // Identificador del usuario (1 o 2)
        color:color,
      });
    },
  },
  computed: {
    dataUsuario1() {
      if (!this.user1) return null;
      return {
        name: `${this.user1.name.first} ${this.user1.name.last}`,
        picture: this.user1.picture.large,
      };
    },
    dataUsuario2() {
      if (!this.user2) return null;
      return {
        name: `${this.user2.name.first} ${this.user2.name.last}`,
        picture: this.user2.picture.large,
      };
    },
  },
  mounted() {
    this.cargarUsuarios();
  },
};
</script>

<template>
  <div class="container mt-5">
    <div class="row">
      <!-- Usuario 1 -->
      <Usuario
        v-if="user1"
        :dataUsuario="dataUsuario1"
        :id="1"
        :nombreUsuario="dataUsuario1.name"
        @enviar-mensaje="agregarMensaje"
      />

      <!--Chat box -->

      <div class="chat-box col-md-6">
        <div class="p-3">
          <div
            v-for="(mensaje, index) in mensajes"
            :key="index"
            class="message mb-3 d-flex"
            :class="
              mensaje.id === 1 ? 'justify-content-start' : 'justify-content-end'
            "
          >
            <div
              class="mensaje-content p-2"
              :style="{
                backgroundColor: mensaje.color, 
                'text-align': mensaje.id === 1 ? 'left' : 'right',
              }"
            >
              <strong>{{
                mensaje.id === 1 ? dataUsuario1.name : dataUsuario2.name
              }}</strong>
              <div>
                {{ mensaje.texto }}
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Usuario 2 -->
      <Usuario
        v-if="user2"
        :dataUsuario="dataUsuario2"
        :id="2"
        :nombreUsuario="dataUsuario2.name"
        @enviar-mensaje="agregarMensaje"
      />
    </div>
  </div>
</template>

<style scoped>
.mensaje-content {
  border-radius: 8px;
  max-width: 60%;
  word-wrap: break-word;
  padding: 10px;
}

.mensaje-usuario1 {
  background-color: lightblue;
}

.mensaje-usuario2 {
  background-color: lightgreen;
}

.message {
  display: flex;
}

.chat-box{
  overflow-y: scroll;
  height: 500px;
  border:2px solid rgb(213, 213, 213);

}
</style>
