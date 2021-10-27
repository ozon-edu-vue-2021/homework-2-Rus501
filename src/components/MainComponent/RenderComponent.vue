<template>
  <div :shown="shown" @click.stop="unfoldFolder($event)" class="wrapper">
    <div class="wrapper-inside">
      <component :is="icon"></component>
      <span>{{ item[propName] }}</span>
    </div>
    <div v-if="isFolder" class="subtree">
      <render-component
        v-for="item in item.contents"
        :item="item"
        :key="item[propName]"
        prop-type="type"
        prop-name="name"
      />
    </div>
  </div>
</template>

<script>
import FolderIcon from '../Icons/FolderIcon.vue'
import FileIcon from '../Icons/FileIcon.vue'
import LinkIcon from '../Icons/LinkIcon.vue'
import EventBus from '../../utils/event-bus'

export default {
  name: 'RenderComponent',
  props: {
    item: {
      type: Object,
      default: () => {},
    },
    propType: {
      type: String,
      default: 'type',
    },
    propName: {
      type: String,
      default: 'name',
    },
  },
  data: () => ({
    shown: false,
  }),
  computed: {
    isFolder() {
      return this.shown && this.item.contents && this.item.contents.length > 0
    },
    icon() {
      switch (this.item[this.propType]) {
        case 'link':
          return LinkIcon
        case 'file':
          return FileIcon
        default:
          return FolderIcon
      }
    },
  },
  methods: {
    unfoldFolder(event) {
      EventBus.$emit('get-path', this.item)
      this.item[this.propType] === 'directory'
        ? (this.shown = !this.shown)
        : window.getSelection().selectAllChildren(event.target)
    },
  },
}
</script>

<style scoped>
.wrapper {
  cursor: pointer;
}

.subtree {
  margin-left: 15px;
}

.wrapper-inside {
  display: flex;
  align-items: center;
  gap: 8px;
}
</style>
