<template>
  <q-page class="flex flex-center">
    <div class="row">
      <q-card class="col-12">
        <img :src="base + year + '/' + pad(month) + '/' + pad(day) + '/' + cam + '/' + num + suffix"
             alt="Webcam-Bild des Bannholzhofs"
             :style="style"
             class="col-12"/>
        <q-card-section>
          <div class="q-pa-md">
            <q-badge>
              Fortlaufende Bildnummer: {{ num }} (0 - 800, Schritt: 1)
            </q-badge>
            <q-slider v-model="num" :min="1" :max="800" :step="1" label/>
          </div>
          <div class="q-pa-md" v-if="base === 'https://bannholzhofcam.imgix.net/'">
            <q-badge>
              Bildbreite (px): {{ width }} (480 - 1920, Schritt: 720)
            </q-badge>
            <q-slider v-model="width" :min="480" :max="1920" :step="720" label/>
          </div>
          <div class="q-pa-md" v-if="base === 'https://bannholzhofcam.imgix.net/'">
            <q-badge>
              Bildqualität: {{ quality }} (10 - 100, Schritt: 30)
            </q-badge>
            <q-slider v-model="quality" :min="10" :max="100" :step="45" label/>
          </div>
          <!--          <div class="q-pa-md">-->
          <!--            <q-badge>-->
          <!--              Verzögerung: {{ delay }} (10 - 100, Schritt: 10)-->
          <!--            </q-badge>-->
          <!--            <q-slider v-model="delay" :min="0.5" :max="5" :step="0.5" label/>-->
          <!--          </div>-->
          <div class="q-pa-md row">
            <q-toggle class="col-12 col-md-4" v-model="autoload" label="Automatik"/>
            <q-select class="q-pa-md col-12 col-md-4" v-model="cam" :options="camOptions" emit-value label="Kamera"/>
            <q-select class="q-pa-md col-12 col-md-4" v-model="base" :options="baseOptions" emit-value label="Quelle"/>
          </div>
        </q-card-section>
      </q-card>
    </div>
  </q-page>
</template>

<script>
import { format } from 'quasar'

const { pad } = format
// TODO: Integrate Date Select, change Imgix to alternative (forgot name, fck), lotta more stuff like API for directories or suncalc and 5-second files instead of increasing file numbers
export default {
  name: 'PageIndex',
  data () {
    return {
      base: 'https://webcam.bannholzhof.at/timelapse/',
      width: 1200,
      quality: 55,
      year: 2021,
      month: 3,
      day: 19,
      cam: 'cam2',
      num: 420,
      autoload: false,
      delay: 1,
      camOptions: [
        { label: 'Süd', value: 'cam2' },
        { label: 'West', value: 'cam1' }
      ],
      baseOptions: [
        { label: 'Webcam', value: 'https://webcam.bannholzhof.at/timelapse/' },
        { label: 'Imgix', value: 'https://bannholzhofcam.imgix.net/' }
      ]
    }
  },
  computed: {
    suffix () {
      return '.jpg?w=' + this.width +
        '&q=' + this.quality +
        '&nr=60' +
        '&nrs=60' +
        '&auto=%27compress,format,enhance%27'
    },
    style () {
      const height = this.width / (16 / 9)
      return {
        width: this.width,
        height: height
      }
    }
  },
  methods: {
    pad (number) {
      return pad(number)
    },
    countUp () {
      if (this.autoload) {
        this.num++
      }
    }
  },
  mounted () {
    setInterval(this.countUp, 1000 * this.delay)
  }
}
</script>
