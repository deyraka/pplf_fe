<template>
  <div class="about">
    <h4 class="pa-6">Details for Kabupaten {{id}}</h4>
    <p>Result = {{id_kabko}}</p>
    <p>ITEMS : {{items}}</p>
    <template>
      <v-data-table
        :headers="headers"
        :items="items"
        :items-per-page="10"
        class="elevation-1"
      ></v-data-table>
    </template>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Details',
  props: ['id'],
  data () {
    return {
      id_kabko: encodeURIComponent(this.id),
      headers: [
        { text: 'Kecamatan', value: 'kec' },
        { text: 'Kelurahan/Desa', value: 'keldes' },
        { text: 'Kode BS', value: 'kode_bs' },
        { text: 'Jumlah Approved', value: 'APPROVED' },
        { text: 'Jumlah Regency Approved', value: 'REGENCY_APPROVED' },
        { text: 'Jumlah Open', value: 'OPEN' }
      ],
      items: null
      // items: [
      //   {
      //     kec: 'fff',
      //     keldes: 'ggg',
      //     kode_bs: 'hhh',
      //     APPROVED: 'JJJ',
      //     REGENCY_APPROVED: 'KKK',
      //     OPEN: 'LLL'
      //   }
      // ]
    }
  },
  mounted () {
    axios
      .post('http://localhost:8000/list/' + this.id_kabko)
      .then(response => { this.items = response.data })
      .catch(e => { console.log(e) })
  }
}
</script>
