<template>
  <q-page class="flex flex-center" >
    <q-input
      v-model="saya"
      filled
      type="textarea"
      rows="20"
      style="width:500px"
    />
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue'
import mqtt from 'mqtt'
import {mqttConfig, urlMQTT} from 'src/lib/mqtt'

export default defineComponent({
  name: 'IndexPage',

  setup() {
    const saya = ref('')
    const client = mqtt.connect(urlMQTT, mqttConfig)
    onMounted(() => {
      client.on('connect', () =>{
        // console.log("Koneksi ke rabbitmq berhasil")

        saya.value='Berhasil  Terhubung ke rabbitmq\n'
      })
      client.subscribe("routing_ryya", (err) =>{
        if (!err) { }
      })

      client.on('message', (topic, payload) =>{
        console.log('testttttttttttt', topic, payload.toString())

        saya.value += `${payload.toString()}\n`

      })

    });

    return {
      saya
    }
  }
})
</script>
