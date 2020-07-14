<template>
  <div>
    <section class="mb-5">
      <v-img
        src="https://21dzk.l.u-tokyo.ac.jp/SAT/image/image01.jpg"
        height="300px"
      >
        <v-row align="center" class="lightbox white--text pa-2 fill-height">
          <v-col>
            <h1 class="display-1 text-center">
              <b>{{ title }}</b>
            </h1>
          </v-col>
        </v-row>
      </v-img>
    </section>

    <v-container>
      <v-btn
        class="mr-1"
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

  get title(): string {
    return this.$store.state.title
  }
}
</script>
