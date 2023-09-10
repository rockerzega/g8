<template>
  <div>
    <div>
      <label>Select Camera:</label>
      <a-select v-model="selectedCamera" @change="handleCameraChange">
        <a-select-option v-for="camera in availableCameras" :key="camera.deviceId" :value="camera.deviceId">
          {{ camera.label }}
        </a-select-option>
      </a-select>
    </div>
    <video ref="videoRef" autoplay playsinline />
    <canvas ref="canvasRef" style="display: none;" />
    <div class="mt-5">
      <a-button @click="captureFrameAndSend">Capturar</a-button>
      <a-button @click="stopCamera">Detener</a-button>
    </div>
    <div v-if="motionDetected">Se movio</div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from 'vue';
import { message } from 'ant-design-vue';

export default {
  name: 'Camara',
  setup(_, ctx) {
    const videoRef = ref(null);
    const canvasRef = ref(null);
    const stream = ref(null);
    const availableCameras = ref([]);
    const selectedCamera = ref(null);
    const motionDetected = ref(false);

    const requestCameraPermission = async () => {
      try {
        const mediaStream = await navigator.mediaDevices.getUserMedia({ video: true });
        videoRef.value.srcObject = mediaStream;
        stream.value = mediaStream;
        loadAvailableCameras();
      } catch (error) {
        console.error('Error accessing camera:', error);
      }
    };

    const loadAvailableCameras = () => {
      navigator.mediaDevices
        .enumerateDevices()
        .then(devices => {
          const cameras = devices.filter(device => device.kind === 'videoinput');
          availableCameras.value = cameras;

          if (cameras.length > 0) {
            selectedCamera.value = cameras[0].deviceId;
            startCamera(cameras[0].deviceId);
          }
        })
        .catch(error => console.error('Error enumerating devices:', error));
    };

    const startCamera = async deviceId => {
      try {
        const constraints = {
          video: { deviceId: { exact: deviceId } }
        };
        const mediaStream = await navigator.mediaDevices.getUserMedia(constraints);
        videoRef.value.srcObject = mediaStream;
        stream.value = mediaStream;
      } catch (error) {
        console.error('Error accessing camera:', error);
      }
    };
    const stopCamera = () => {
      if (stream.value) {
        stream.value.getTracks().forEach(track => track.stop());
        stream.value = null;
        ctx.emit('isStop')
      }
    };

    const captureFrameAndSend = async () => {
      const canvas = canvasRef.value;
      const video = videoRef.value;
      canvas.width = video.videoWidth;
      canvas.height = video.videoHeight;

      const context = canvas.getContext('2d');
      context.drawImage(video, 0, 0, canvas.width, canvas.height);

      const imageData = canvas.toDataURL('image/jpeg');
      // await sendFrameToAPI(imageData);
    };
    const sendFrameToAPI = async frameData => {
      try {
        const response = await fetch('http://localhost:3001/upload', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ frameData })
        });

        if (response.ok) {
          console.log('Frame enviado');
        } else {
          console.error('Error al enviar el frame a la API');
        }
      } catch (error) {
        console.error('Error al enviar el frame:', error);
      }
    };

    onMounted(() => {
      requestCameraPermission();
    });

    onUnmounted(() => {
      if (stream.value) {
        stream.value.getTracks().forEach(track => track.stop());
      }
    });

    return {
      videoRef,
      canvasRef,
      availableCameras,
      selectedCamera,
      motionDetected,
      stopCamera,
      captureFrameAndSend,
      handleCameraChange: event => {
        selectedCamera.value = event;
        startCamera(event);
      }
    };
  }
};
</script>

<style>
/* Estilos si es necesario */
</style>