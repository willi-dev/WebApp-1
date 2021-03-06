<template>
  <div class="filter-item has-text-centered" @click.prevent="$emit('toggle')">
    <hc-button circle 
               size="small" 
               :color="itemState"
               :disabled="disabled"
               :class="{
                 'active': active,
                 'disabled': disabled
               }">
      <slot name="icon">
        <hc-icon v-if="icon" :set="iconSet" :icon="icon" />
      </slot>
    </hc-button>
    <small>
      <slot name="title">
        {{ titleText }}
      </slot>
    </small>
  </div>
</template>

<script>
  export default {
    name: 'hc-filter-item',
    props: {
      item: {
        type: Object,
        default: () => {}
      },
      title: {
        type: String,
        required: true
      },
      translationPath: {
        type: String
      },
      slug: {
        type: String
      },
      icon: {
        type: String,
        required: true
      },
      iconSet: {
        type: String,
        default: 'hc'
      },
      active: {
        type: Boolean,
        default: false
      },
      disabled: {
        type: Boolean,
        default: false
      }
    },
    computed: {
      itemState () {
        return this.active ? 'success' : 'disabled'
      },
      titleText () {
        if (!this.translationPath) {
          return this.title
        }
        if (this.slug) {
          return this.$t(this.translationPath + this.slug, this.title)
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import "assets/styles/utilities";

  .filter-item {
    display: inline-block;
    width: 8rem;
    height: 6rem;
    vertical-align: bottom;
    cursor: pointer;
    margin-bottom: 1.5rem;

    button.button {
      font-size: 1.2em;
      margin-bottom: 0.6rem;

      img.emoji {
        transform: scale(1.8);
      }
    }

    small {
      display: inline-block;
      font-size: 0.8em;
      line-height: 1.2em;
      height: 3em;
      width: 100%;
      color: $grey;
      vertical-align: top;
      padding-left: 2px;
      padding-right: 2px;
    }

    @include until($tablet) {
      width: 48%;
      height: auto;
      margin-bottom: 0.5rem;
      display: inline-flex;
      align-items: center;
      padding: 0.3em;
      background-color: $white-ter;
      border-radius: 10em;

      &:nth-child(2n) {
        margin-left: 2%;
      }

      button.button {
        flex: 0 0 2.25em;
        min-width: 2.25em;
        max-width: 2.25em;
        margin-bottom: 0;
        margin-right: 0.2rem;
      }

      small {
        height: auto;
        text-align: left;
        flex: 1 1 0;
      }
    }
  }
</style>
