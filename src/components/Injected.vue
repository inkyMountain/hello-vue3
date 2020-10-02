<template>
  
</template>

<script lang="ts">
import Vue, { inject, provide } from 'vue'

export default {
  name: 'injected',

  setup() {
    inject('testProvide')
  }
}
</script>