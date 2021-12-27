<template>
  <div class="about">
    <h4 class="pa-6">Details for Blok Sensus {{bs}}</h4>
    <p class="px-6">Kecamatan : {{kec}}</p>
    <p class="px-6">Kelurahan/Desa : {{kel}}</p>
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
        <v-spacer></v-spacer>
        <download-excel
          :data="json_data"
          :fields="json_fields"
          worksheet="My Worksheet"
          :header="getTitle(id)"
          :name="getFileName(id)"
        >
          <v-btn
            icon
            color="success"
            fab outlined small
          >
            <v-icon
              title="Export to Excel"
            >
              mdi-file-excel
            </v-icon>
          </v-btn>
        </download-excel>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="baskets"
        :items-per-page="10"
        :loading="loading"
        :search="search"
        sort-by="no_urut_ruta"
        class="elevation-1"
      >
        <template v-slot:item.status="{ item }">
          <v-chip
            :color="getColor(item.status)"
            dark
          >
            {{ item.status }}
          </v-chip>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Details',
  props: ['kec', 'kel', 'bs'],
  data () {
    return {
      // id_kabko: encodeURIComponent(this.bs),
      search: '',
      loading: true,
      headers: [
        { text: 'No Urut Ruta', value: 'no_urut_ruta' },
        { text: 'Kecamatan', value: 'kec' },
        { text: 'Kelurahan/Desa', value: 'keldes' },
        { text: 'Kode Identitas', value: 'kode_identitas' },
        { text: 'Nama KK', value: 'nama' },
        { text: 'Status Pencacahan', value: 'status' }
      ],
      // headers: [
      //   { text: 'Name', value: 'name' },
      //   { text: 'Country', value: 'country' },
      //   { text: 'Website', value: 'website' }
      // ],
      baskets: [],
      json_fields: {
        Kecamatan: 'kec',
        'Kelurahan/Desa': 'keldes',
        'Kode Identitas': 'kode_identitas',
        'No Urut Ruta': 'no_urut_ruta',
        'Nama KK': 'nama',
        'Status Pencacahan': 'status'
      },
      json_data: []
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
      .post('https://ics62-api.bpskalteng.web.id/detailbs/' + this.bs)
      // .post('http://localhost/pplf_api/public/detailbs/' + this.bs)
      .then((response) => {
        this.loading = false
        this.json_data = response.data
        this.baskets = response.data
      })
      .catch((e) => { console.log(e) })
    // axios
    //   .get('https://api.instantwebtools.net/v1/airlines')
    //   .then((response) => { this.baskets = response.data })
    //   .catch((e) => { console.log(e) })
  },

  methods: {
    getColor (stts) {
      if (stts === 'APPROVED') return 'green'
      else if (stts === 'REGENCY APPROVED') return 'orange'
      else if (stts === 'OPEN') return 'grey'
      else return 'red'
    },
    getFileName (val) {
      return 'BS ' + this.bs
    },
    getTitle (val) {
      return 'Daftar Sampel PPLF Blok Sensus : ' + this.bs
    }
  }
}
</script>
