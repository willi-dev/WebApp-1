<template>
  <div>
    <div class="heading shout">
      <hc-tooltip :label="$t('component.shout.addShout')">
        <hc-button circle
                    size="large"
                    @click="handleShout"
                    :color="buttonColor"
                    style="font-size: 2em; margin-bottom: 0.8rem;">
          <hc-icon set="fa" :icon="icon" />
        </hc-button>
      </hc-tooltip>
      <br/>
      {{ $t('component.shout.buttonLabel') }}
    </div>
    <div class="title" style="font-size: 1.5rem; margin-top: -0.5rem;">
      {{ shoutCount }}
    </div>
  </div>
</template>

<script>
  export default {
    name: 'hc-shout-button',
    data () {
      return {
        shoutCount: 0,
        shoutId: null
      }
    },
    props: {
      contribution: {
        type: Object,
        required: true
      },
      user: {
        type: Object
      }
    },
    watch: {
      'contribution.shoutCount' (shoutCount) {
        this.shoutCount = shoutCount
      }
    },
    computed: {
      buttonColor () {
        return this.shoutId ? 'success' : 'default'
      },
      icon () {
        return this.shoutId ? 'check' : 'bullhorn'
      }
    },
    methods: {
      async handleShout () {
        try {
          if (this.shoutId) {
            await this.$api.service('shouts').remove(this.shoutId)
            this.updateStatus(null)
          } else {
            const shout = await this.$api.service('shouts').create({
              userId: this.user._id,
              foreignId: this.contribution._id,
              foreignService: 'contributions'
            })
            this.updateStatus(shout._id)
          }
          if (this.shoutId) {
            this.$snackbar.open({
              message: this.$t('component.shout.messageSuccess'),
              duration: 4000,
              type: 'is-success'
            })
          }
        } catch (err) {
          this.update()
        }
      },
      updateStatus (shoutId) {
        this.shoutId = shoutId
      },
      update () {
        if (!this.user) {
          return
        }
        // see if user has shouted this out
        this.$api.service('shouts').find({
          query: {
            userId: this.user._id,
            foreignId: this.contribution._id
          }
        }).then(res => this.updateStatus(res.total ? res.data[0]._id : null))
      }
    },
    created () {
      this.shoutCount = this.contribution.shoutCount
    },
    mounted () {
      this.update()
    }
  }
</script>

<style lang="scss" scoped>

</style>
