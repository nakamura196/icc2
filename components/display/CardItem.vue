<template>
  <v-card no-body class="mb-4">
    <nuxt-link
      :to="
        localePath({
          name: 'item',
          query: { id: item._id, u: $route.query.u },
        })
      "
    >
      <v-img
        :src="$utils.formatArrayValue(item._source._thumbnail)"
        contain
        style="height: 150px;"
        width="100%"
        class="grey lighten-2"
      ></v-img>
    </nuxt-link>

    <div class="pa-4">
      <nuxt-link
        :to="
          localePath({
            name: 'item',
            query: { id: item._id, u: $route.query.u },
          })
        "
        class="mr-2"
      >
        <!-- eslint-disable-next-line vue/no-v-html -->
        <b v-html="$utils.formatArrayValue(item._source._label)"></b>
      </nuxt-link>
      <template v-if="item.access">
        <div class="mt-2">
          {{ item.access }}
        </div>
      </template>
    </div>

    <template v-if="!item.share_hide">
      <v-divider />

      <v-card-actions>
        <v-spacer></v-spacer>
        <ResultOption :item="item" />
      </v-card-actions>
    </template>
  </v-card>
</template>

<script lang="ts">
import { Vue, Prop, Component } from 'nuxt-property-decorator'
import ResultOption from '~/components/display/ResultOption.vue'
// import { queryStore } from '~/store'

@Component({
  components: {
    ResultOption,
  },
})
export default class cardItem extends Vue {
  @Prop({
    default: 240,
  })
  width!: number

  @Prop({
    default: 300,
  })
  height!: number

  @Prop({ required: true })
  item!: any

  @Prop({
    default: false,
  })
  horizontal!: boolean

  get advanced() {
    if (this.$store.state.mode === 'all') {
      return true
    } else {
      return false
    }
  }

  removeItem(id: string, type: string) {
    if (type === 'id') {
      this.$store.commit('removeId', [id])
      // queryStore.removeId([id])
    } else {
      this.$store.commit('removeImage', [id])
      // queryStore.removeImage([id])
    }

    this.$router.push(
      this.localePath({
        name: 'search',
        query: this.$utils.getSearchQueryFromQueryStore(this.$store.state),
      }),
      () => {},
      () => {}
    )
  }
}
</script>
<style>
a {
  text-decoration: none;
}
</style>
