<template>
  <div
    @keyup.arrow-up.stop="moveFocusUp"
    @keyup.arrow-down.stop="moveFocusDown"
    @keyup.enter.stop="unfoldFolder"
    tabindex="0"
    :shown="shown"
    @click.stop="unfoldFolder"
    class="wrapper"
  >
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
        :path="`${path}/${item.name}`"
        @select="(value) => $emit('select', value)"
      />
    </div>
  </div>
</template>

<script>
import FolderIcon from '../Icons/FolderIcon.vue'
import FileIcon from '../Icons/FileIcon.vue'
import LinkIcon from '../Icons/LinkIcon.vue'

// prevent page scrolling with arrow keys
document.addEventListener(
  'keydown',
  (e) => {
    if (['ArrowUp', 'ArrowDown'].indexOf(e.code) > -1) {
      e.preventDefault()
    }
  },
  false
)

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
    path: {
      type: String,
      default: '/',
    },
  },
  data: () => ({
    shown: false,
  }),
  computed: {
    isFolder() {
      return this.shown && this.item.contents
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
  directives: {
    // focus: {
    //   inserted(el) {
    //     el.focus()
    //   },
    // },
  },
  methods: {
    unfoldFolder(event) {
      this.$emit('select', this.path)

      this.item[this.propType] === 'directory'
        ? (this.shown = !this.shown)
        : window.getSelection().selectAllChildren(event.target)
    },
    moveFocusUp(e) {
      const shown = e.target.getAttribute('shown')
      const unfolded = !e.target.querySelector('.wrapper')
      if (shown && unfolded) {
        e.target.querySelector('.wrapper').focus()
      } else {
        const prevFolder = e.target.previousElementSibling
        prevFolder
          ? prevFolder.focus()
          : e.target.parentElement.parentElement.focus()
      }
    },
    moveFocusDown(e) {
      const shown = e.target.getAttribute('shown')
      if (shown) {
        e.target.querySelector('.wrapper').focus()
      } else {
        const nextElem = e.target.nextElementSibling
        if (nextElem) {
          e.target.nextElementSibling.focus()
        } else {
          let parentHasNextSibling =
            e.target.parentElement.parentElement.nextElementSibling

          const traverseUp = (elem) => {
            let parent = elem.parentElement.parentElement
            while (!parent.nextElementSibling) {
              parent = parent.parentElement.parentElement
            }
            return parent.nextElementSibling
          }

          parentHasNextSibling
            ? parentHasNextSibling.focus()
            : traverseUp(e.target).focus()
        }
      }
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
