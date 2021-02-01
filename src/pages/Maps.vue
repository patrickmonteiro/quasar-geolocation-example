<template>
  <q-page padding>
    <l-map
      v-if="ready"
      style="height: 90vh"
      :zoom="zoom"
      :center="center"
    >
      <l-tile-layer :url="url"></l-tile-layer>
      <l-marker :lat-lng="markerLatLng" ></l-marker>
      <l-marker
        v-for="(picole, index) in picoleLatLng"
        :key="index"
        :lat-lng="[picole.lat, picole.log]"
        :icon="icon"
      >
        <l-popup>
          {{ picole.info }}
        </l-popup>
      </l-marker>
    </l-map>
  </q-page>
</template>

<script>
import L from 'leaflet'
export default {
  name: 'PageMaps',
  data () {
    return {
      latitude: '',
      longitude: '',
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      zoom: 20,
      center: [],
      markerLatLng: [],
      picoleLatLng: [],
      ready: false,
      icon: L.icon({
        iconUrl: 'picole3.png',
        iconSize: [50, 50],
        iconAnchor: [16, 37]
      })
    }
  },
  mounted () {
    this.getGeolocation()
  },
  methods: {
    getGeolocation () {
      if (navigator.geolocation) {
        this.$q.loading.show()
        navigator.geolocation.getCurrentPosition(this.setPosition, this.errorPosition)
      } else {
        this.errorPosition()
      }
    },
    setPosition (position) {
      const coords = position.coords
      this.center = [coords.latitude, coords.longitude]
      this.markerLatLng = [coords.latitude, coords.longitude]
      this.picoleLatLng = [
        {
          lat: (coords.latitude + 0.0005),
          log: (coords.longitude + 0.0005),
          info: 'Kibom'
        },
        {
          lat: (coords.latitude + 0.0008),
          log: (coords.longitude + 0.0008),
          info: 'Nestle'
        },
        {
          lat: (coords.latitude + -0.0005),
          log: (coords.longitude + -0.0005),
          info: 'Magnu'
        },
        {
          lat: (coords.latitude + 0.0010),
          log: (coords.longitude + -0.0013),
          info: 'Limão'
        }
      ]
      this.$q.loading.hide()
      this.successNotify()
      this.ready = true
    },
    errorPosition () {
      this.$q.notify({
        position: 'bottom',
        timeout: 3000,
        color: 'negative',
        textColor: 'white',
        actions: [{ icon: 'close', color: 'white' }],
        message: 'Não foi possível recupera sua posição!'
      })
      this.$q.loading.hide()
    },
    successNotify () {
      this.$q.notify({
        position: 'bottom',
        timeout: 3000,
        color: 'positive',
        textColor: 'white',
        actions: [{ icon: 'check', color: 'white' }],
        message: 'Posição recuperada com sucesso!'
      })
    }
  }
}
</script>
