<template>
  <!-- 本组件将由uni-provider插件自动混入到每一个页面中 -->
  <MescrollBody
    :up="{ use: enableUp }"
    :down="{ use: enableDown }"
    @init="mescroll = $event"
    @up="fetch"
    @down="enableUp ? $event.resetUpScroll() : fetch($event)"
  >
    <slot></slot>
  </MescrollBody>
  <slot name="fixed"></slot>
</template>

<script setup lang="ts">
  import MescrollBody from 'mescroll-uni/mescroll-body.vue'
  import { useQuery, ScrollSymbol, ScrollOptions } from '@/hooks'
  const { t } = $(useQuery())

  onUnload(() => uni.$off(t)) // 页面卸载,解绑回调事件

  const { mescroll, fetch, enable } = $(inject<ScrollOptions>(ScrollSymbol) || ({} as any))
  let enableUp = $computed(() => ['all', 'up'].some((e) => e == enable))
  let enableDown = $computed(() => ['all', 'down'].some((e) => e == enable))
</script>

<style></style>
