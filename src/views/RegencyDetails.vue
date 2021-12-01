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
        sort-by="calories"
        class="elevation-1"
      >
        <template v-slot:item.actions="{ item }">
          <v-btn
            icon
            color="primary"
            :to="'/bs-details/'+item.kec+'/'+item.keldes+'/'+item.kode_bs"
          >
              <v-icon
                small
                class="ma-2"
              >
                mdi-eye
              </v-icon>
          </v-btn>
        </template>
        <template v-slot:no-data>
          <p>No Data Found</p>
        </template>
      </v-data-table>
    </v-card>
    <!-- <h5>{{baskets}}</h5> -->
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Details',
  props: ['id'],
  data: () => ({
    search: '',
    loading: true,
    headers: [
      { text: 'Kecamatan', value: 'kec' },
      { text: 'Kelurahan/Desa', value: 'keldes' },
      { text: 'Kode BS', value: 'kode_bs' },
      { text: 'Jumlah Approved', value: 'APPROVED' },
      { text: 'Jumlah Regency Approved', value: 'REGENCY_APPROVED' },
      { text: 'Jumlah Open', value: 'OPEN' },
      { text: 'Actions', value: 'actions', sortable: false }
    ],
    baskets: []
  }),

  created () {
    this.initialize()
  },

  methods: {
    initialize () {
      axios
        .post('http://10.62.6.21:8000/list/' + encodeURIComponent(this.id))
        .then((response) => {
          this.loading = false
          this.baskets = response.data
        })
        .catch((e) => { console.log(e) })
    }
  }
}
</script>
