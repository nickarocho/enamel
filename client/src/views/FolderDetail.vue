<template>
  <div class="white card max-height">
    <div class="folder-header">
      <form @submit.prevent="updateFolder">
        <input class="no-outline header-title folder-name" type="text" name="taskname" ref="taskname"
          v-model="folderName" @keyup.esc="cancel">
        </input>
      </form>
    </div>
  </div>
</template>
<script>
import { UpdateFolder } from '../constants/query.gql'
export default {
  data() {
    return {
      folderName: this.folder.name,
    }
  },
  props: ['folder'],
  mounted() {
    document.title = `${this.folder.name} - enamel`
  },
  methods: {
    updateFolder(e) {
      const name = this.folderName
      if (name === this.folder.name) {
        this.cancel(e)
        return
      }
      this.$apollo.mutate({
        mutation: UpdateFolder,
        variables: { id: this.folder.id, input: {name} },
      }).then(() => {
        this.cancel(e)
      }).catch((error) => {
        console.log(error)
      })
    },
    cancel(e) {
      e.target.blur()
    }
  }
}
</script>