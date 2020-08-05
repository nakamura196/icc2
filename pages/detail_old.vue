<template>
  <div>
    <v-sheet color="grey lighten-3">
      <v-container class="py-4">
        <h1>
          {{ label }}
        </h1>
      </v-container>
    </v-sheet>

    <v-container>
      <v-row class="pa-4 mb-5">
        <v-col cols="12" sm="3" class="mb-4">
          <v-img
            :src="image"
            contain
            style="height: 150px;"
            width="100%"
            class="grey lighten-2"
          ></v-img>
        </v-col>
        <v-col cols="12" sm="9">
          <p>{{ description }}</p>

          <p>
            <v-btn
              color="primary"
              :to="
                localePath({
                  name: 'search',
                  query: {
                    u: $route.query.u,
                    'fc-Creator': label,
                  },
                })
              "
            >
              {{ $t('search') }}
            </v-btn>
          </p>
        </v-col>
      </v-row>
    </v-container>

    <v-container fluid>
      <div v-for="(obj, index) in result" :key="index" cols="12" sm="4">
        <h3 class="mb-4">{{ obj.parent['rdfs:label'][0]['@value'] }}</h3>
        <HorizontalEntities :key="obj.parent['@id']" :data="obj.children" />
      </div>
    </v-container>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Watch } from 'nuxt-property-decorator'
import axios from 'axios'
import HorizontalEntities from '~/components/display/HorizontalEntities.vue'
@Component({
  components: {
    HorizontalEntities,
  },
})
export default class PageCategory extends Vue {
  @Watch('$route', { deep: true })
  watchTmp(): void {
    this.init()
  }

  image: string = ''
  label: string = ''
  description: string = ''

  async init() {
    // const routeQuery = context.query

    this.result = []

    let label: any = this.$route.query.field
    if (label === '_label') {
      label = this.$t(label)
    }

    label = 'schema:creator'

    const uri = this.$route.query.uri
    const data: any = await this.api2(uri)
    this.image = data['schema:image'][0]['@id']
    this.label = data['rdfs:label'][0]['@value']
    if (data['schema:description'] && data['schema:description'].length > 0) {
      this.description = data['schema:description'][0]['@value']
    }

    for (const key in data) {
      if (
        key !== 'schema:description' &&
        key !== 'rdfs:label' &&
        key !== '@id' &&
        key !== '@context' &&
        key !== 'schema:image'
      ) {
        this.api(key, data[key][0]['@id'])
      }
    }
  }

  head() {
    return {
      title: this.$t('category') + ' : ' + this.$t(this.label),
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.$t('search_by_category'),
        },
      ],
    }
  }

  mounted() {
    this.init()
  }

  getQuery(arr: any[]): { [key: string]: string } {
    const result: { [key: string]: string } = {}
    for (let i = 0; i < arr.length; i++) {
      const obj = arr[i]
      result[obj.field] = obj.value
    }
    return result
  }

  search() {
    const query: any = JSON.parse(JSON.stringify(this.$route.query)) // 重要
    query['size-fc'] = this.size
    this.$router.push(
      this.localePath({
        name: 'category-slug',
        params: { slug: this.title },
        query,
      }),
      () => {},
      () => {}
    )
  }

  initSizeFc() {
    const query = JSON.parse(JSON.stringify(this.$route.query)) // 重要
    query['size-fc'] = '50'
    return query
  }

  result: any = []

  async api2(uri) {
    const endpoint: string = 'http://localhost:8008/api.json'
    const result = await axios.get(endpoint).then(function (data) {
      const result = data.data
      for (let i = 0; i < result.length; i++) {
        const obj = result[i]
        if (obj['@id'] === uri) {
          return obj
        }
      }
    })
    return result
  }

  api(property, uri) {
    const endpoint: string = 'http://localhost:8008/api.json'
    const self = this
    axios.get(endpoint).then(function (data) {
      const result = data.data
      let parent: any = null
      for (let i = 0; i < result.length; i++) {
        const obj = result[i]
        if (obj['@id'] === uri) {
          parent = obj
        }
      }

      const children: any[] = []

      for (let i = 0; i < result.length; i++) {
        const obj = result[i]
        const values = obj[property]
        if (values && values.length > 0) {
          if (
            values[0]['@id'] === uri &&
            obj['@id'] !== self.$route.query.uri
          ) {
            const newObj = {
              _id: obj['@id'],
              _source: {
                _thumbnail: [obj['schema:image'][0]['@id']],
                _label: [obj['rdfs:label'][0]['@value']],
                _description: [obj['schema:description'][0]['@value']],
              },
            }
            children.push(newObj)
          }
        }
      }

      self.result.push({
        parent,
        children,
      })
    })
  }
}
</script>
