<template>
    <div class="pa-6">
        <v-container>
            <v-row
                :justify="center"
            >
                <v-col
                    v-for="item in items"
                    :key="item.id_kabko"
                >
                    <v-card
                        class="mx-auto my-4"
                        color="#26c6da"
                        dark
                        max-width="400"
                    >
                        <v-card-title>
                        <v-icon
                            large
                            left
                        >
                            mdi-map-marker-radius
                        </v-icon>
                        <span class="text-h6 font-weight-light">Kabupaten</span>
                        </v-card-title>

                        <v-card-text class="text-h5 font-weight-bold">
                        {{item.name}}
                        </v-card-text>

                        <v-card-actions>
                        <v-list-item class="grow">
                            <v-row
                            align="center"
                            justify="end"
                            >
                                <router-link :to="'/details/'+item.id_kabko">
                                    <v-btn
                                    depressed
                                    color="primary"
                                    >
                                        Pilih
                                    </v-btn>
                                </router-link>
                            </v-row>
                        </v-list-item>
                        </v-card-actions>
                    </v-card>
                </v-col>
            </v-row>
        </v-container>
    </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Regency',
  data () {
    return {
      items: null
    }
  },
  mounted () {
    axios
      .get('http://localhost:8000/kabko')
      .then(response => { this.items = response.data })
      .catch(e => { console.log(e) })
  }

}
</script>
