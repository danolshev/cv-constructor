<template>
  <div class="container column">
    <app-form @add-block="addBlock"></app-form>
    <div class="card card-w70">
      <component v-model="blocks" v-for="block in blocks" :key="block.id" :is="'cv-' + block.type" v-bind="block.data"></component>
      <h3 v-if="!blocks.length">Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>
  <app-comments></app-comments>
</template>

<script>
import AppComments from "@/components/AppComments";
import AppForm from "@/components/AppForm";
import CvAvatar from "@/components/blocks/CvAvatar";
import CvTitle from "@/components/blocks/CvTitle";
import CvSubtitle from "@/components/blocks/CvSubtitle";
import CvText from "@/components/blocks/CvText";
export default {
  data() {
    return {
      blocks: []
    }
  },
  mounted() {
    this.getBlocks()
  },
  methods: {
    async getBlocks() {
      const response = await fetch('https://cv-constructor-3e29b-default-rtdb.europe-west1.firebasedatabase.app/blocks.json', {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json'
        }
      })

      const blocks = await response.json()
      if (blocks) {
        this.blocks = Object.keys(blocks).map(id => {
          return {
            id: id,
            type: blocks[id].type,
            data: blocks[id].data
          }
        })
      }
    },
    async addBlock(block) {
      let data = {}
      switch (block.type) {
        case 'title':
          data = {title: block.data}
          break
        case 'subtitle':
          data = {subtitle: block.data}
          break
        case 'avatar':
          data = {avatar: block.data}
          break
        case 'text':
          data = {text: block.data}
          break
      }

      const newBlock = {type: block.type, data: data}
      const id = await this.saveBlock(newBlock)
      this.blocks.push(Object.assign(newBlock, {id: id.name}))

    },
    async saveBlock(newBlock) {
      try {
        const response = await fetch('https://cv-constructor-3e29b-default-rtdb.europe-west1.firebasedatabase.app/blocks.json', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(newBlock)
        })
        return await response.json()
      } catch (e) {
        console.warn(e)
      }
    }
  },
  components: {CvText, CvSubtitle, CvTitle, CvAvatar, AppForm, AppComments}
}
</script>
