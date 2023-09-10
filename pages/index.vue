<template>
  <div>
    <h1 class="font-bold text-center italic text-3xl text-blue-400" padding:1 >Detección de movimientos</h1>
      <div class="flex lg:flex-row flex-col justify-center gap-x-6">
        <div class="p-2 ">
          <a-card class="transition-all border-gray-100"  style="width: 240px" @click="sentado">
            <img
              slot="cover"
              alt="example"
              src="https://static.vecteezy.com/system/resources/previews/004/221/969/non_2x/dog-mascot-sitting-free-vector.jpg"
            />
            <a-card-meta class="text-center" title="Mascota sentada">
              <template slot="description">
                Movimiento a detectar
              </template>
            </a-card-meta>
          </a-card>
          
          <a-card class="transition-all" style="width: 240px" @click="correcto">
            <img
              slot="cover"
              alt="example"
              src="https://img.freepik.com/vector-premium/perro-acostado-concepto-entrenamiento-mascotas-ilustracion-vectorial_605517-357.jpg?w=740"/>
            <a-card-meta class="text-center" title="Mascota acostada">
              <template slot="description">
                Movimiento a detectar
              </template>
            </a-card-meta>
          </a-card>
        </div>

        <div class="p-2">
          <Camara v-if="camaraVisible" @isStop="camaraVisible=false" />
          <a-card v-else>
            <img
              slot="cover"
              alt="example"
              src="https://png.pngtree.com/png-vector/20220514/ourlarge/pngtree-camara-icon-design-png-image_4630479.png"
            />
          </a-card>
        </div>

        <div class="p-2 ">
          <a-card class="transition-all" style="width: 240px" @click="fallido">
            <img
              slot="cover"
              alt="example"
              src="https://static.vecteezy.com/system/resources/previews/019/550/823/original/cartoon-illustration-of-a-jack-russell-terrier-the-dog-is-standing-sideways-with-head-raised-isolated-on-a-blue-background-pets-animals-dog-theme-a-design-element-in-a-flat-style-vector.jpg"
            />
            <a-card-meta class="text-center" title="Mascota quieta">
              <template slot="description">
                Movimiento a detectar
              </template>
            </a-card-meta>
          </a-card>
          
          <a-card class="hover:shadow-2xl hover:scale-105 transition-all" hoverable style="width: 240px" >
            <img
              slot="cover"
              alt="example"
              src="https://static.vecteezy.com/system/resources/previews/007/637/359/non_2x/photo-camera-zone-black-silhouette-icon-notice-photography-allowed-area-alert-camera-capture-zone-place-red-green-circle-symbol-no-photograph-camera-sign-isolated-illustration-vector.jpg"/>
            <a-card-meta class="text-center text-2xl" title="Orden detectada">
              <template slot="description">
                <h1 class="text-green-500">
                Orden detectada
              </h1>
              </template>
             
            </a-card-meta>
          </a-card>
        </div>
      </div>
      <a-button @click="camaraVisible=true">Encender camara</a-button> 
  </div>
</template>

<script>
import moment from 'moment'
import { Howl } from 'howler'
import Camara from '../components/camara.vue'

import { message } from 'ant-design-vue'
export default {
  components: {
    Camara,
  },
  layout: 'home',
  name: 'IndexPage',
  data() {
    return {
      camaraVisible: false,
      isMoving: false,
      audiofile: '/audio/acostado.mp3',
      listaAudios: ['acostado', 'quieto', 'sentado'],
    }
  },
  head() {
    return {
      title: 'Tareas'
    }
  },
  mounted() {
    // this.webcam = new VueWebcam('#webcam')
    // this.webcam.start()
    // this.webcam.on('motion', () => {
    //   console.log('algo se movio')
    //   this.isMoving = true
    // })
  },
  methods: {
    mostrar(message) {
      console.log(message)
    },
    detener() {
      console.log("se envio a detener")
      this.camaraVisible = false
    },
    sentado() {
      console.log("se envio a sentado")
      const orden = Math.floor(Math.random() * 3)
      const sound = new Howl({
        src: [`/audio/${this.listaAudios[orden]}.mp3`], // Ruta a tu archivo de audio
      })
      sound.play()
    },
    correcto() {
      message.info('Has obtenido un premio')
      console.log("se envio a correcto")
    },
    fallido() {
      message.error('No obtienes un premio')
      console.log("se envio a fallido")
    },
  }
}
</script>