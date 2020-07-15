<template>
  <div>
    <v-container class="mt-0 mb-0">
      <v-row class="mt-5">
        <v-col cols="12" sm="8">
          <v-row>
            <v-col cols="12" :sm="6">
              <v-img
                contain
                align="center"
                :src="$utils.formatArrayValue(metadata._source._thumbnail)"
                height="300"
                class="grey lighten-2"
                @click="overlay = !overlay"
            /></v-col>
            <v-col cols="12" :sm="6">
              <h2>
                <b>{{ $utils.formatArrayValue(metadata._source._label) }}</b>
              </h2>

              <div class="mt-5">
                <a
                  v-for="(value, index) in metadata._source._license"
                  :key="index"
                  class="mr-2"
                  :href="value"
                  target="_blank"
                  >{{ metadata._source._license[index] }}
                  <v-icon>mdi-open-in-new</v-icon>
                </a>
              </div>

              <div class="mt-5">
                <v-tooltip bottom>
                  <template v-slot:activator="{ on }">
                    <v-btn
                      v-if="metadata._source._manifest"
                      class="grey lighten-3 ma-1"
                      :href="
                        'http://pocket.cultural.jp' +
                        ($i18n.locale == 'ja' ? '' : '/en') +
                        '/?url=' +
                        $utils.formatArrayValue(
                          metadata._source._manifest
                        ) /* +
                        '&related=' +
                        url*/
                      "
                      target="_blank"
                      v-on="on"
                    >
                      {{ $t('save_to_my_list') }}
                    </v-btn>
                  </template>
                  <span>{{ $t('save_to_iiif_pocket') }}</span>
                </v-tooltip>
              </div>
            </v-col>
          </v-row>
          <template v-if="metadata._source._related">
            <v-btn
              target="_blank"
              :href="$utils.formatArrayValue(metadata._source._related)"
              class="primary ma-1"
            >
              {{ $t('view_full_item') }}
              <v-icon>mdi-open-in-new</v-icon>
            </v-btn>
          </template>

          <v-btn
            v-if="metadata._source._curation"
            target="_blank"
            :href="
              'http://codh.rois.ac.jp/software/iiif-curation-viewer/demo/?curation=' +
              $utils.formatArrayValue(metadata._source._curation) +
              '&pos=' +
              $utils.formatArrayValue(metadata._source._pos)
            "
            class="primary ma-1"
          >
            IIIF Curation Viewer
            <v-icon>mdi-open-in-new</v-icon>
          </v-btn>
        </v-col>

        <v-col cols="12" sm="4">
          <v-sheet
            v-if="metadata._source._manifest"
            class="grey lighten-3 py-5 px-3 py-3"
            style="background-color: #f9f6f0;"
          >
            <IIIFViewers
              :manifest="$utils.formatArrayValue(metadata._source._manifest)"
            />
          </v-sheet>

          <v-sheet
            class="grey lighten-3 py-5 px-3 py-3"
            style="background-color: #f9f6f0;"
          >
            <share-buttons
              :title="title"
              :url="url"
              :image="$utils.formatArrayValue(metadata._source._thumbnail)"
              :manifest="$utils.formatArrayValue(metadata._source._manifest)"
            />
          </v-sheet>
        </v-col>
      </v-row>
    </v-container>

    <v-container>
      <template v-for="(obj, field) in metadata._source">
        <dl v-if="!field.startsWith('_')" :key="field" class="row mt-0">
          <dt class="col-sm-3 text-muted">{{ field }}</dt>
          <dd class="col-sm-9">
            <template>
              <div v-for="(value, index) in obj" :key="index">
                <nuxt-link
                  :to="
                    localePath({
                      name: 'search',
                      query: $utils.createFacetQuery([
                        { field: 'fc-' + field, value: value },
                        { field: 'u', value: [$route.query.u] },
                      ]),
                    })
                  "
                  >{{ value }}</nuxt-link
                >
              </div>
            </template>
          </dd>
        </dl>
      </template>
    </v-container>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'nuxt-property-decorator'
import ShareButtons from '~/components/common/ShareButtons.vue'
import IIIFViewers from '~/components/item/IIIFViewers.vue'

@Component({
  components: {
    ShareButtons,
    IIIFViewers,
  },
})
export default class Item extends Vue {
  metadata: any = null

  tab: string = ''

  uri: string = ''
  url: string = ''
  title: string = ''
  id: string = ''

  async fetch(context: any) {
    const store = context.store
    const state = store.state

    if (state.index == null) {
      const uri = `${context.query.u}`
      const index = await context.app.$searchUtils.createIndexFromIIIFCurationList(
        uri
      )
      store.commit('setIndex', index.index)
      store.commit('setData', index.data)
      store.commit('setTitle', index.title)
    }
  }

  created() {
    const itemId = this.$route.query.id
    const data = this.$store.state.data
    for (let i = 0; i < data.length; i++) {
      const id = data[i]._id
      if (id === itemId) {
        this.metadata = data[i]
      }
    }
    this.url = location.href
  }
}
</script>
