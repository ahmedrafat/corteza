<template>
  <div class="d-inline-flex">
    <template v-if="!inConfirmation">
      <b-button
        :variant="ctaClass"
        :disabled="disabled"
        class="confirmation-prompt flex-fill"
        @click.prevent="onPrompt"
      >
        <slot />
      </b-button>
    </template>

    <template v-if="inConfirmation">
      <b-button
        :variant="confirmationClass"
        class="confirmation-confirm flex-fill mr-1"
        @click.prevent="onConfirmation()"
      >
        {{ $t('label.yes') }}
      </b-button>

      <b-button
        variant="secondary"
        class="confirmation-cancel flex-fill"
        @click.prevent="inConfirmation=false"
      >
        {{ $t('label.no') }}
      </b-button>
    </template>
  </div>
</template>
<script>
export default {
  props: {
    ctaClass: { type: String, default: 'danger' },
    confirmationClass: {
      default: 'danger',
      type: String,
    },
    disabled: Boolean,
    noPrompt: Boolean,
  },

  data () {
    return {
      inConfirmation: false,
    }
  },

  i18nOptions: {
    namespaces: 'admin',
    keyPrefix: 'general',
  },

  methods: {
    onPrompt () {
      if (this.noPrompt) {
        this.$emit('confirmed')
      } else {
        this.inConfirmation = true
      }
    },

    onConfirmation () {
      this.inConfirmation = false
      this.$emit('confirmed')
    },
  },
}
</script>
<style scoped lang="scss">
.btn {
  margin: 0 1px;
}

.btn-url {
  color: var(--danger);
  text-decoration: none;

  &:hover {
    color: var(--danger);

    .icon-trash {
      font-weight: 900;
    }
  }
}
</style>
