<template>
  <h2>{{subtitle}}</h2>
</template>

<script>
export default {
  name: "AppCvSubtitle",
  props: {
    subtitle: {
      type: String,
      required: true,
    }
  }
}
</script>

<style scoped>

</style>