<template>
  <form class="card card-w30" @submit.prevent="addBlock">
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select v-model="type" id="type">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>
    </div>

    <div class="form-control">
      <label for="value">Значение</label>
      <textarea v-model="text" id="value" rows="3"></textarea>
    </div>

    <button class="btn primary" :disabled="disabledSubmit">Добавить</button>
  </form>
</template>

<script>
export default {
  name: "AppForm",
  emits: {
    'add-block'(data) {
      if (data) {
        return true
      }
      console.warn('add-block lost data')
      return false
    }
  },
  data() {
    return {
      type: 'title',
      text: '',
    }
  },
  computed: {
    disabledSubmit() {
      return this.text.length < 4
    }
  },
  methods: {
    addBlock() {
      this.$emit('add-block', {type: this.type, data: this.text})
      this.type = 'title'
      this.text = ''
    }
  }
}
</script>

<style scoped>

</style>