<template>
  <q-page>
    <div class="row justify-center">
      <div
        class="text-center"
        :class="$q.screen.lt.sm ? 'col-8' : 'col-4'"
      >
        <lottie-player
          src="https://assets9.lottiefiles.com/packages/lf20_jqpju2hc.json"
          background="transparent"
          speed="1"
          loop
          autoplay
        ></lottie-player>
      </div>
    </div>
    <div class="row justify-center">
      <div class="col-10 text-center">
        <p>
          Latitude: {{ latitude }}
        </p>
        <p>
          Longitude: {{ longitude }}
        </p>
      </div>
    </div>
    <div class="row justify-center q-mt-lg">
      <q-btn
        :class="$q.screen.lt.sm ? 'col-8' : 'col-4'"
        color="indigo"
        label="Minha localizacao"
        push
        size="lg"
        @click="getGeolocation"
      />
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      latitude: '',
      longitude: ''
    }
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
      this.latitude = coords.latitude
      this.longitude = coords.longitude
      this.$q.loading.hide()
      this.successNotify()
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
