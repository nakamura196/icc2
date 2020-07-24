<template>
  <div>
    <section class="mb-5 grey">
      <v-img
        :src="image"
        height="300px"
        gradient="to top, rgba(0, 0, 0,.33), rgba(0, 0, 0,.33)"
      >
        <v-row align="center" class="lightbox white--text pa-2 fill-height">
          <v-col>
            <h3 v-if="attribution" class="text-center mb-5">
              {{ attribution }}
            </h3>
            <h1 class="display-1 text-center">
              <b>{{ title }}</b>
            </h1>
            <!-- 
            <div class="text-center mt-10">
              <v-btn
                dark
                color="primary"
                :to="
                  localePath({
                    name: 'search',
                    query: {
                      u: $route.query.u,
                    },
                  })
                "
              >
                <v-icon>mdi-magnify</v-icon> {{ $t('search') }}
              </v-btn>
            </div>
            -->
          </v-col>
        </v-row>
      </v-img>
    </section>

    <v-container>
      <p v-if="description">{{ description }}</p>

      <p>
        <v-row>
          <v-col>
            <v-card>
              <v-card-text>
                <p class="display-1 text--primary">
                  {{ $t('search') }}
                </p>
                <p>
                  {{ $t('desc_search') }}
                </p>
              </v-card-text>
              <v-card-actions>
                <v-btn
                  color="primary"
                  :to="
                    localePath({
                      name: 'search',
                      query: {
                        u: $route.query.u,
                      },
                    })
                  "
                >
                  {{ $t('view') }}
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
          <v-col>
            <v-card>
              <v-card-text>
                <p class="display-1 text--primary">
                  {{ $t('category') }}
                </p>
                <p>
                  {{ $t('desc_category') }}
                </p>
              </v-card-text>
              <v-card-actions>
                <v-btn
                  color="primary"
                  :to="
                    localePath({
                      name: 'category',
                      query: {
                        u: $route.query.u,
                      },
                    })
                  "
                >
                  {{ $t('view') }}
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
          <v-col>
            <v-card>
              <v-card-text>
                <p class="display-1 text--primary">
                  {{ $t('tree') }}
                </p>
                <p>
                  {{ $t('desc_tree') }}
                </p>
              </v-card-text>
              <v-card-actions>
                <v-btn
                  color="primary"
                  :to="
                    localePath({
                      name: 'tree',
                      query: {
                        u: $route.query.u,
                      },
                    })
                  "
                >
                  {{ $t('view') }}
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </p>
    </v-container>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'

@Component
export default class Page extends Vue {
  head() {
    return {
      titleTemplate: null,
    }
  }

  image: string = this.$store.state.thumbnail ? this.$store.state.thumbnail : ''

  get title(): string {
    return this.$store.state.title
  }

  get thumbnail(): string {
    return this.$store.state.thumbnail
  }

  get attribution(): string {
    return this.$store.state.attribution
  }

  get description(): string {
    return this.$store.state.description
  }

  async fetch(context: any) {
    const store = context.store
    const state = store.state

    if (state.index == null) {
      const uri = context.query.u
      if (!uri) {
        location.href =
          'https://github.com/nakamura196/icc2/blob/master/README.md'
      }
      const index = await context.app.$searchUtils.createIndex(uri)
      store.commit('setIndex', index.index)
      store.commit('setData', index.data)
      store.commit('setTitle', index.title)
      store.commit('setThumbnail', index.thumbnail)
      store.commit('setDescription', index.description)
      store.commit('setAttribution', index.attribution)
    }
  }
}
</script>
