<template>
  <div :shown="shown" @click.stop="unfoldFolder($event)" class="wrapper">
    <component :is="icon"></component>
    {{ item[propName] }}
    <div
      v-if="shown && item.contents && item.contents.length > 0"
      class="subtree"
    >
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
    contents: {
      type: Array,
      default: () => [],
    },
  },
  data: () => ({
    shown: false,
  }),
  computed: {
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
</style>
