<template>
  <q-page class="flex flex-center">
    <div class="row">
      <q-card class="col-12">
        <img :src="base + year + '/' + pad(month) + '/' + pad(day) + '/' + cam + '/' + num + suffix"
             alt="Webcam-Bild des Bannholzhofs"
             class="col-12"/>
        <q-card-section>
          <q-slider v-model="num" :min="1" :max="800" :step="1" label-always/>
          <q-slider v-model="width" :min="640" :max="1920" :step="80" label-always/>
          <q-slider v-model="quality" :min="10" :max="100" :step="10" label-always/>
        </q-card-section>
      </q-card>
    </div>
  </q-page>
</template>

<script>
import { format } from 'quasar'

const { pad } = format

export default {
  name: 'PageIndex',
  data () {
    return {
      base: 'https://bannholzhofcam.imgix.net/',
      width: 1280,
      quality: 90,
      year: 2021,
      month: 3,
      day: 19,
      cam: 'cam2',
      num: 420
    }
  },
  computed: {
    suffix () {
      return '.jpg?w=' + this.width +
        '&q=' + this.quality +
        '&nr=60' +
        '&nrs=60' +
        '&auto=%27compress,format,enhance%27'
    }
  },
  methods: {
    pad (number) {
      return pad(number)
    }
  }
}
</script>
