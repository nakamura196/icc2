<template>
  <v-card
    no-body
    class="mb-4"
    :style="horizontal ? 'width: ' + width + 'px;' : ''"
  >
    <nuxt-link
      :to="
        localePath({
          name: 'detail',
          query: {
            uri: item._id,
            u: $route.query.u,
            //field: $route.query.field,
          },
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

    <div
      class="pa-4"
      :style="
        horizontal
          ? 'width: ' +
            width +
            'px; height: ' +
            /*height*/ 100 +
            'px; overflow-y: auto;'
          : ''
      "
    >
      <nuxt-link
        :to="
          localePath({
            name: 'detail',
            query: {
              uri: item._id,
              u: $route.query.u,
              //field: $route.query.field,
            },
          })
        "
        class="mr-2"
      >
        <!-- eslint-disable-next-line vue/no-v-html -->
        <b v-html="$utils.formatArrayValue(item._source._label)"></b>
      </nuxt-link>
      <template v-if="item._source._description">
        <div class="mt-2">
          {{ $utils.formatArrayValue(item._source._description) }}
        </div>
      </template>
    </div>

    <template v-if="false">
      <v-divider />

      <v-card-actions>
        <v-spacer></v-spacer>
        <ResultOption
          :item="{
            label: $utils.formatArrayValue(item._source._label),
            manifest: $utils.formatArrayValue(item._source._manifest),
            url: encodeURIComponent(getUrl(item)),
          }"
        />
      </v-card-actions>
    </template>
  </v-card>
</template>

<script lang="ts">
import { Vue, Prop, Component } from 'nuxt-property-decorator'
import ResultOption from '~/components/display/ResultOption.vue'

@Component({
  components: {
    ResultOption,
  },
})
export default class CardItem extends Vue {
  @Prop({ required: true })
  item!: any

  @Prop({
    default: 240,
  })
  width!: number

  @Prop({
    default: 300,
  })
  height!: number

  @Prop({
    default: false,
  })
  horizontal!: boolean

  getUrl(item: any) {
    return (
      process.env.BASE_URL +
      this.localePath({
        name: 'item',
        query: {
          u: this.$route.query.u,
          id: item._id,
        },
      })
    )
  }
}
</script>
<style>
a {
  text-decoration: none;
}
</style>
