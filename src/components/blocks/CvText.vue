<template>
  <p>{{text}}</p>
</template>

<script>
export default {
  name: "AppCvText",
  props: {
    text: {
      type: String,
      required: true,
    }
  }
}
</script>

<style scoped>

</style>