<template>
  <div
    class="position-relative"
  >
    <ace-editor
      v-model="expressionValue"
      :lang="lang"
      :mode="lang"
      theme="chrome"
      width="100%"
      :height="height"
      :class="{ 'border': border }"
      v-on="$listeners"
      @init="editorInit"
    />

    <b-button
      v-if="showPopout"
      variant="link"
      class="popout position-absolute px-2 py-1"
      @click="$emit('open')"
    >
      <font-awesome-icon
        :icon="['fas', 'expand-alt']"
      />
    </b-button>
  </div>
</template>

<script>
import AceEditor from 'vue2-ace-editor'

export default {
  components: {
    AceEditor,
  },

  props: {
    value: {
      type: String,
      default: '',
    },

    lang: {
      type: String,
      default: 'text',
    },

    height: {
      type: String,
      default: '80',
    },

    showLineNumbers: {
      type: Boolean,
      default: false,
    },

    fontSize: {
      type: String,
      default: '14px',
    },

    border: {
      type: Boolean,
      default: true,
    },

    showPopout: {
      type: Boolean,
      default: true,
    },
  },

  computed: {
    expressionValue: {
      get () {
        return this.value
      },

      set (value = '') {
        this.$emit('update:value', value)
      },
    },
  },

  methods: {
    editorInit (editor) {
      require('brace/mode/text')
      require('brace/mode/javascript')
      require('brace/theme/chrome')

      editor.setOptions({
        tabSize: 2,
        fontSize: this.fontSize,
        wrap: true,
        indentedSoftWrap: false,
        showLineNumbers: this.showLineNumbers,
        showGutter: this.showLineNumbers,
        displayIndentGuides: this.lang !== 'text',
        useWorker: false,
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.border {
  background-color: var(--white);
  border: 2px solid var(--light);
  border-radius: 0.25rem;
}

.popout {
  z-index: 7;
  bottom: 0;
  right: 0;
}
</style>
