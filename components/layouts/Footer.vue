<template>
  <v-footer :dark="true" class="mt-5">
    <v-container>
      <p class="text-center my-5">
        <small>
          <a style="color: white;" href="https://github.com/nakamura196/icc2"
            >https://github.com/nakamura196/icc2</a
          >
        </small>
      </p>
    </v-container>
  </v-footer>
</template>

<script lang="ts">
import { Vue, Component } from 'nuxt-property-decorator'

@Component
export default class Footer extends Vue {}
</script>
