<template>
  <div class="about">
    <h4 class="pa-6">Details for Kabupaten {{id}}</h4>
    <v-card
      class="pa-2 mx-6 mb-6"
    >
      <v-card-title>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search ..."
          single-line
          hide-details
        >
        </v-text-field>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="baskets"
        :items-per-page="10"
        :loading="loading"
        :search="search"
        class="elevation-1"
      >
        <template v-slot:item.actions="{ item }">
          <router-link :to="'/regency-details/'+item">
              <v-icon
                small
                class="ma-2"
              >
                mdi-pencil
              </v-icon>
          </router-link>
        </template>
      </v-data-table>
    </v-card>
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
      search: '',
      loading: true,
      headers: [
        { text: 'Kecamatan', value: 'kec' },
        { text: 'Kelurahan/Desa', value: 'keldes' },
        { text: 'Kode BS', value: 'kode_bs' },
        { text: 'Jumlah Approved', value: 'APPROVED' },
        { text: 'Jumlah Regency Approved', value: 'REGENCY_APPROVED' },
        { text: 'Jumlah Open', value: 'OPEN' }
      ],
      // headers: [
      //   { text: 'Name', value: 'name' },
      //   { text: 'Country', value: 'country' },
      //   { text: 'Website', value: 'website' }
      // ],
      baskets: []
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
      .post('http://10.62.6.21:8000/list/' + this.id_kabko)
      .then((response) => {
        this.loading = false
        this.baskets = response.data
      })
      .catch((e) => { console.log(e) })
    // axios
    //   .get('https://api.instantwebtools.net/v1/airlines')
    //   .then((response) => { this.baskets = response.data })
    //   .catch((e) => { console.log(e) })
  }
}
</script>
