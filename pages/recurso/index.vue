<template>
  <div>
    <web-cam ref="webcam" :width="videoWidth" :height="videoHeight" :autoplay="true" @started="onWebcamStarted" />
    <p v-if="isMoving">Se detectó movimiento</p>
  </div>
</template>

<script>
import { WebCam } from 'vue-web-cam';

export default {
  components: {
    WebCam,
  },
  data() {
    return {
      isMoving: false,
      videoWidth: 640, // Ancho del video de la cámara
      videoHeight: 480, // Alto del video de la cámara
    };
  },
  methods: {
    onWebcamStarted() {
      // Una vez que la cámara se haya iniciado, comenzamos a detectar movimiento
      this.startMotionDetection();
    },
    startMotionDetection() {
      const videoElement = this.$refs.webcam.video;

      // Configura un intervalo para verificar el movimiento cada ciertos milisegundos
      const motionDetectionInterval = setInterval(() => {
        if (this.detectMotion(videoElement)) {
          // Se detectó movimiento
          this.isMoving = true;
        } else {
          // No se detectó movimiento
          this.isMoving = false;
        }
      }, 1000); // Verificar cada segundo (ajusta el valor según tus necesidades)
    },
    detectMotion(videoElement) {
      // Aquí puedes implementar tu lógica de detección de movimiento
      // Puedes utilizar bibliotecas como tracking.js o cualquier otro método de tu elección
      // Para este ejemplo, simplemente devolvemos true para simular la detección de movimiento
      return true; // Simulación de detección de movimiento
    },
  },
};
</script>
