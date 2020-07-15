<template>
  <div>
    {{ fields }}
    <v-data-table :headers="headers" :items="items" class="elevation-1">
      <template v-slot:item.calories="{ item }">
        <v-chip :color="getColor(item.calories)" dark>{{
          item.calories
        }}</v-chip>
      </template>
    </v-data-table>
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">Name</th>
            <th class="text-left">Calories</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in desserts" :key="item.name">
            <td>{{ item.name }}</td>
            <td>{{ item.calories }}</td>
            <td>{{ item.calories }}</td>
            <td>{{ item.calories }}</td>
            <td>{{ item.calories }}</td>
            <td>{{ item.calories }}</td>
            <td>{{ item.calories }}</td>
            <td>{{ item.calories }}</td>
            <td>{{ item.calories }}</td>
            <td>{{ item.calories }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
    <div style="overflow-x: auto;">
      <v-simple-table style="width: 100%;">
        <template v-slot:default>
          <thead>
            <tr>
              <th v-for="(obj, index) in fields" :key="index" class="text-left">
                {{ obj.label }}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, key) in results" :key="key">
              <td>
                <nuxt-link
                  class="mb-4"
                  :to="
                    localePath({
                      name: 'item',
                      query: { id: item.id },
                    })
                  "
                >
                  <v-img
                    :src="$utils.formatArrayValue(item._source._thumbnail)"
                    contain
                    style="height: 90px; width: 90px;"
                    class="grey lighten-2 my-2"
                  ></v-img>
                </nuxt-link>
              </td>
              <td>
                <nuxt-link
                  :to="
                    localePath({
                      name: 'item',
                      query: { id: item.id },
                    })
                  "
                >
                  <!-- eslint-disable-next-line vue/no-v-html -->
                  <span
                    v-html="$utils.formatArrayValue(item._source._label)"
                  ></span>
                </nuxt-link>
              </td>
              <td
                v-for="(fieldObj, fieldIndex) in fields.slice(
                  2,
                  fields.length - 2
                )"
                :key="fieldIndex"
                width="10%"
              >
                {{ $utils.formatArrayValue(item._source[fieldObj.key]) }}
              </td>

              <td width="10%">
                <ResultOption v-if="!item.share_hide" :item="item" />
              </td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'nuxt-property-decorator'
import ResultOption from '~/components/display/ResultOption.vue'

@Component({
  components: {
    ResultOption,
  },
})
export default class TableSearchResult extends Vue {
  get results() {
    return this.$store.state.result.hits.hits
  }

  desserts = [
    {
      name: 'Frozen Yogurt',
      calories: 159,
      calories2: 159,
      calories3: 159,
    },
    {
      name: 'Ice cream sandwich',
      calories: 237,
    },
  ]

  get query() {
    return this.$store.state.query
  }

  items: any[] = []
  fields!: any[]

  headers: any[] = []

  created() {
    const facetLabels: any = this.$store.state.facetLabels
    const fields: any = [
      { key: 'image', label: '' },
      { key: 'label', label: this.$t('title') },
    ]
    for (const field in facetLabels) {
      fields.push({
        key: field,
        label: field,
      })
    }
    fields.push({ key: 'icons', label: '' })
    this.fields = fields

    const headers = []
    for (let i = 0; i < fields.length; i++) {
      const field = fields[i]
      headers.push({
        text: field.label,
        align: 'start',
        value: field.key,
      })
    }
    this.headers = headers

    const results = this.results
    const items: any[] = []
    for (let i = 0; i < results.length; i++) {
      const item: any = {}
      for (let j = 0; j < fields.length; j++) {
        const label = fields[j].key
        item[label] = this.$utils.formatArrayValue(item._source[label])
      }
      items.push(item)
    }
    this.items = items
  }
}
</script>
