<template>
  <div>
    <v-sheet color="grey lighten-3">
      <v-container class="py-4">
        <h1>
          {{ $t('tree') }}
        </h1>
      </v-container>
    </v-sheet>

    <v-container fluid>
      <v-row>
        <v-col cols="12" md="4">
          <div :style="'max-height: ' + height + 'px'" class="overflow-y-auto">
            <v-treeview :items="items">
              <template v-slot:label="{ item }">
                <span>
                  <template v-if="item['type'] == 'sc:Manifest'">
                    <template
                      v-if="
                        $vuetify.breakpoint.name != 'xs' &&
                        $vuetify.breakpoint.name != 'sm'
                      "
                    >
                      <a @click="select(item.id)">{{ item.name }}</a>
                    </template>
                    <template v-else>
                      <nuxt-link
                        :to="
                          localePath({
                            name: 'item',
                            query: {
                              u: $route.query.u,
                              id: item.id,
                            },
                          })
                        "
                        >{{ item.name }}</nuxt-link
                      >
                    </template>
                  </template>
                  <template v-else>
                    {{ item.name }}
                  </template>
                </span>
              </template>
            </v-treeview>
          </div>
        </v-col>
        <v-col cols="12" md="8">
          <div :style="'max-height: ' + height + 'px'" class="overflow-y-auto">
            <Metadata v-if="metadata" :metadata="metadata" />
          </div>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Watch } from 'nuxt-property-decorator'
import Metadata from '~/components/item/Metadata.vue'
// import axios from 'axios'

@Component({
  components: {
    // ShareButtons,
    // IIIFViewers,
    Metadata,
  },
})
export default class PageTree extends Vue {
  @Watch('$route', { deep: true })
  watchTmp(): void {
    this.init()
  }

  height: number = -1

  results: any[] = []
  label: string = ''
  title: string = ''

  size: number = -1

  map: string[] = []

  items: any[] = []

  id: string = ''

  metadata: any = null

  async init() {
    this.height = window.innerHeight * 0.95

    const store = this.$store
    const state = store.state

    if (state.json == null) {
      const uri: any = this.$route.query.u
      const index = await this.$searchUtils.createIndex(uri)
      store.commit('setIndex', index.index)
      store.commit('setData', index.data)
      store.commit('setTitle', index.title)
      store.commit('setThumbnail', index.thumbnail)
      store.commit('setDescription', index.description)
      store.commit('setAttribution', index.attribution)
      store.commit('setJson', index.json)
    }

    const collection = this.$store.state.json
    const items = this.rec(collection).children
    this.items = items
  }

  rec(/* items, */ collection: any) {
    const children = collection.collections || collection.manifests

    const obj: any = {
      id: collection['@id'],
      name: collection.label,
      type: collection['@type'],
    }

    if (collection['@type'] === 'sc:Manifest') {
      obj.url = this.localePath({
        name: 'item',
        query: {
          u: this.$route.query.u,
          id: collection['@id'],
        },
      })
    }

    if (collection.thumbnail) {
      obj.thumbnail = collection.thumbnail
    }

    if (children == null) {
      return obj
    }

    const newChildren = []

    for (let i = 0; i < children.length; i++) {
      const child = children[i]

      const obj2 = this.rec(/* newChildren, */ child)

      newChildren.push(obj2)
    }

    if (newChildren.length > 0) {
      obj.children = newChildren
    }
    return obj
  }

  head() {
    return {
      title: this.$t('collection'),
    }
  }

  select(itemId: string) {
    const data = this.$store.state.data
    for (let i = 0; i < data.length; i++) {
      const id = data[i]._id
      if (id === itemId) {
        this.metadata = data[i]
      }
    }
  }

  mounted() {
    this.init()
  }
}
</script>
