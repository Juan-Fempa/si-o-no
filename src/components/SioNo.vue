<template>
  <!-- <img src="https://via.placeholder.com/250" alt="fondo" /> -->
  <img v-if="urlImg" :src="urlImg" alt="fondo" />
  <!-- Fondo que oscurece toda la pantalla -->
  <div class="bg-dark"></div>
  <div class="indecision-container">
    <input type="text" placeholder="¿Qué quieres saber?" v-model="pregunta" />
    <p>Recuerda terminar con un signo de interrogación (?).</p>
    <div>
      <h2>{{ preguntaCorrecta }}</h2>
      <h1>{{ respuesta }}</h1>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      pregunta: "",
      preguntaCorrecta: null,
      respuesta: null,
      urlImg: null,
    };
  },
  methods: {
    async obtenerRespuesta() {
      this.respuesta = "Pensando...";
      const { answer, image } = await fetch("https://yesno.wtf/api").then(
        (respuesta) => respuesta.json()
      );
      this.respuesta = answer;
      this.urlImg = image;
    },
  },
  watch: {
    pregunta(nuevoValor) {
      if (!nuevoValor.endsWith("?")) return;
      this.preguntaCorrecta = nuevoValor;
      this.obtenerRespuesta();
    },
  },
};
</script>

<style>
img,
.bg-dark {
  height: 100vh;
  left: 0px;
  max-height: 100%;
  max-width: 100%;
  position: fixed;
  top: 0px;
  width: 100vw;
}
.bg-dark {
  background-color: rgba(0, 0, 0, 0.4);
}
.indecision-container {
  position: relative;
  z-index: 99;
}
input {
  border-radius: 5px;
  border: none;
  padding: 10px 15px;
  width: 250px;
}
input:focus {
  outline: none;
}
p {
  color: white;
  font-size: 20px;
  margin-top: 0px;
}
h1,
h2 {
  color: white;
}
h2 {
  margin-top: 150px;
}
</style>