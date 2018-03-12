<template>
  <div class="project-plugins-add page">
    <div class="content">
      <StepWizard
        title="Add a plugin"
        class="frame"
        :tab-id.sync="tabId"
      >
        <template slot-scope="{ next, previous }">
          <VueTab
            id="search"
            label="Search"
            icon="search"
            disabled
          >
            <div class="content vue-disable-scroll">
              <ais-index
                app-id="OFCNCOG2CU"
                api-key="db283631f89b5b8a10707311f911fd00"
                index-name="npm-search"
                :query-parameters="{
                  hitsPerPage: 10,
                  attributesToRetrieve: [
                    'name',
                    'description',
                    'repository',
                    'homepage',
                    'version',
                    'owner',
                    'humanDownloadsLast30Days'
                  ],
                  attributesToHighlight: [
                    'name',
                    'description'
                  ]
                }"
              >
                <InstantSearchInput/>
                <ais-results>
                  <PackageSearchItem
                    slot-scope="{ result }"
                    :pkg="result"
                    :selected="selectedId === result.name"
                    @click.native="selectedId = result.name"
                  />
                </ais-results>
                <InstantSearchPagination/>
              </ais-index>
            </div>

            <div class="actions-bar no-padding-x">
              <VueButton
                icon-left="close"
                label="Cancel"
                class="big"
                @click="cancel()"
              />

              <div class="algolia">
                <img class="ais-logo" src="~@/assets/search-by-algolia.svg">
              </div>

              <VueButton
                icon-left="add"
                :label="`Install ${selectedId || 'plugin'}`"
                class="big primary"
                :disabled="!selectedId"
                @click="installPlugin()"
              />
            </div>
          </VueTab>

          <VueTab
            id="config"
            label="Configuration"
            icon="settings_applications"
            disabled
            lazy
          >
            <div class="content vue-disable-scroll">
              <PromptsList
                :prompts="enabledPrompts"
                @answer="answerPrompt"
              />
            </div>

            <div class="actions-bar no-padding-x">
              <VueButton
                icon-left="arrow_back"
                label="Cancel"
                class="big"
                @click="showCancelInstall = true"
              />

              <VueButton
                icon-left="done"
                label="Finish installation"
                class="big primary"
                :disabled="!configurationValid"
                @click="invokePlugin()"
              />
            </div>
          </VueTab>
        </template>
      </StepWizard>
    </div>

    <VueModal
      v-if="showCancelInstall"
      :title="`Uninstall ${selectedId}?`"
      class="small"
      @close="showCancelInstall = false"
    >
      <div class="default-body">
        Do you want to uninstall the {{ selectedId }} plugin?
      </div>

      <div slot="footer" class="actions space-between">
        <VueButton
          label="Go back"
          class="flat"
          @click="showCancelInstall = false"
        />

        <VueButton
          label="Cancel without uninstalling"
          class="flat"
          @click="cancelInstall()"
        />

        <VueButton
          label="Uninstall"
          icon-left="delete_forever"
          class="danger"
          @click="uninstallPlugin()"
        />
      </div>
    </VueModal>
  </div>
</template>

<script>
export default {
  name: 'ProjectPluginsAdd',

  data () {
    return {
      tabId: 'search',
      selectedId: null,
      enabledPrompts: [],
      showCancelInstall: false
    }
  },

  computed: {
    configurationValid () {
      return false
    }
  },

  methods: {
    cancel () {
      this.$router.push({ name: 'project-home' })
    },

    installPlugin () {
      // TODO
      this.tabId = 'config'
    },

    cancelInstall ()  {
      this.selectedId = null
      this.tabId = 'search'
      this.showCancelInstall = false
    },

    uninstallPlugin () {
      // TODO
      this.cancelInstall()
    },

    invokePlugin () {
      // TODO
    },

    answerPrompt () {
      // TODO
    }
  }
}
</script>

<style lang="stylus" scoped>
@import "~@/style/imports"

.project-plugins-add
  display grid
  grid-template-columns 1fr
  grid-template-rows auto
  grid-template-areas "content"

.content
  grid-area content

.algolia
  position absolute
  margin 0 auto
  top 0
  left 0
  width 100%
  height 100%
  pointer-events none
  h-box()
  box-center()
</style>