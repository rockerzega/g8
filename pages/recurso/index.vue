<template>
  <div id="app">
    <vue-webcam id="webcam" />
    <p v-if="isMoving">Se detectó movimiento</p>
  </div>
</template>

<script>
import VueWebcam from 'vue-webcam';
import { message } from 'ant-design-vue'

export default {
  components: {
    VueWebcam,
  },
  data() {
    return {
      isMoving: false,
      orden: '',
      listaAudios: ['acostado', 'quieto', 'sentado'],
      audioFile: '',
    };
  },
  mounted() {
    this.webcam = new VueWebcam('#webcam');
    this.webcam.start();

    this.webcam.on('motion', () => {
      this.isMoving = true;
      this.audios()
    });
    const frames = [];
    const v = this
setInterval(async (v) => {
  const frame = v.webcam.capture();


  frames.push(frame);
  if (frames.length === 4) {
    const data = {
      frames: frames,
    };

    v.resultado( await axios.post('https://inf-7add3120-788f-44ba-a66b-6576397d3514-no4xvrhsfq-uc.a.run.app/detect', data))
    frames = [];
  }
}, 1000);
  },
  methods: {
    audios() {
      const orden = Math.floor(Math.random() * 3)
      this.orden = this.listaAudios[orden]
      const sound = new Howl({
        src: [`/audio/${this.listaAudios[orden]}.mp3`], // Ruta a tu archivo de audio
      })
      sound.play()
    },
    async resultado(data) {
      let prob = 0
      for (const item of data) {
        if (item.label === this.orden) {
          prob++
        }
      }
      if (prob >= 2) {
        await axios.get('http://localhost:5000/premio')
        message.info('Has obtenido un premio')
      } else {
        message.error('No obtienes un premio')
      }
    }
  }
};
</script>