<template>
  <div>
    <Metadata v-if="metadata" :metadata="metadata" />
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'nuxt-property-decorator'
import Metadata from '~/components/item/Metadata.vue'

@Component({
  components: {
    Metadata,
  },
})
export default class Item extends Vue {
  metadata: any = null

  tab: string = ''

  uri: string = ''
  url: string = location.href
  title: string = ''
  id: string = ''

  async fetch(context: any) {
    const store = context.store
    const state = store.state

    if (state.index == null) {
      const uri = context.query.u
      const index = await context.app.$searchUtils.createIndex(uri)
      store.commit('setIndex', index.index)
      store.commit('setData', index.data)
      store.commit('setTitle', index.title)
      store.commit('setThumbnail', index.thumbnail)
      store.commit('setDescription', index.description)
      store.commit('setAttribution', index.attribution)
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
  }

  /*
  sorted(source: any) {
    const obj: any = {}
    const keys = Object.keys(source) // .sort()
    for (let i = 0; i < keys.length; i++) {
      const key = keys[i]
      obj[key] = source[key]
    }
    return obj
  }
  */
}
</script>
