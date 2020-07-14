<template>
  <div>
    <v-simple-table fixed-header>
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

  get query() {
    return this.$store.state.query
  }

  items: any[] = []
  fields!: any[]

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
  }
}
</script>
