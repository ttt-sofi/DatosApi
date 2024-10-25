<template>
  <div class="col-md-3 ">
    <img :src="dataUsuario.picture" alt="Imagen del Usuario" class="" 
    />
    <br />
    <div class="label-color">
      <label for="color">Elige tu color</label>
      <input id="color" type="color" v-model="colorMensaje" class="mt-2 mb-2 form-control form-control-color" />
      <br />
    </div>

    <label :for="'message' + id">{{ nombreUsuario }}</label> <br />
    <textarea type="text" :id="'message' + id" v-model="mensajeTemporal" placeholder="Escribe un mensaje"
      class="form-control mt-2"></textarea>
    <button @click="enviarMensaje" class="btn btn-primary mt-2">Enviar</button>
  </div>
</template>

<script>
export default {
  props: {
    dataUsuario: Object,
    id: Number, // id único para el input del mensaje
    nombreUsuario: String,
  },
  data() {
    return {
      mensajeTemporal: "",
      colorMensaje: "#ADD8E6",
    };
  },
  methods: {
    enviarMensaje() {
      if (this.mensajeTemporal.trim() !== "") {
        this.$emit("enviar-mensaje", {
          texto: this.mensajeTemporal,
          id: this.id,
          color: this.colorMensaje,
        });

        this.mensajeTemporal = ""; // Limpiar el input después de enviar
      } else {
        alert("Por favor, escribe un mensaje antes de enviarlo.");
      }
    },
  },
};
</script>

<style scoped>

.label-color{
  display: flex;
  align-items: center;
  gap: 20px;
}

</style>
