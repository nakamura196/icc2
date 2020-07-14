<template>
  <v-row>
    <v-col cols="12" sm="3" class="mb-4">
      <nuxt-link
        class="mb-4"
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
    </v-col>
    <v-col cols="12" sm="9">
      <h3 class="mb-2">
        <nuxt-link
          :to="
            localePath({
              name: 'item',
              query: { id: item._id, u: $route.query.u },
            })
          "
        >
          <!-- eslint-disable-next-line vue/no-v-html -->
          <span v-html="$utils.formatArrayValue(item._source._label)"></span>
        </nuxt-link>
      </h3>

      <template v-for="(obj, field) in item._source">
        <dl v-if="!field.startsWith('_')" :key="field" class="row my-0 py-0">
          <dt class="col-sm-3 my-0 py-0">
            <b>{{ field }}</b>
          </dt>
          <dd class="col-sm-9 my-0 py-0">
            <template>
              <span v-for="(value, index) in obj" :key="index" class="mr-4">
                {{ $utils.truncate(value, 50) }}
              </span>
            </template>
          </dd>
        </dl>
      </template>

      <div class="text-right my-4">
        <a
          v-if="item._source._related"
          class="mr-2 primary--text"
          :href="$utils.formatArrayValue(item._source._related)"
          target="_blank"
        >
          {{ $t('view_full_item') }}
          <v-icon>mdi-open-in-new</v-icon>
        </a>

        <ResultOption v-if="!item.share_hide" :item="item" />
      </div>
    </v-col>
  </v-row>
</template>

<script lang="ts">
import { Vue, Prop, Component } from 'nuxt-property-decorator'
import ResultOption from '~/components/display/ResultOption.vue'

@Component({
  components: {
    ResultOption,
  },
})
export default class ListItem extends Vue {
  @Prop()
  item!: any
}
</script>
