<template>
  <div>
    <nuxt-link :to="item.path ? localePath(item.path) : null">
      <v-img
        :src="item.image"
        contain
        style="height: 200px;"
        width="100%"
        class="grey lighten-2"
      ></v-img>
    </nuxt-link>
  </div>
</template>

<script lang="ts">
import { Vue, Prop, Component } from 'nuxt-property-decorator'

@Component
export default class ThumbItem extends Vue {
  @Prop({ required: true })
  item!: any
}
</script>
