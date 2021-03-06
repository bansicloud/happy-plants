<template>
  <div id="app">
    <app-notifications
      class="notifications"
      :message="message" />

    <app-header :scroll-up="true"
      :transparent="transparent"
      :color="iconColor"
      :settings="settingsBtn"
      :settings-icon="settingsIcon"
      :settings-on-click="settingsBtnOnClick"
      :back-button="backBtn"
      :back-path="backBtnPath">
      <h1 v-if="!!pageTitle" slot="title">
        {{ pageTitle }}
      </h1>
    </app-header>

    <router-view />
  </div>
</template>

<script>
  import { mapActions, mapState } from 'vuex'
  import AppHeader from '@/components/AppHeader'
  import AppNotifications from '@/components/AppNotifications'

  export default {
    name: 'HappyPlants',

    meta: {
      title: 'Happy Plants'
    },

    components: {
      'app-notifications': AppNotifications,
      'app-header': AppHeader
    },

    data () {
      return {
        notificationTimeout: 2000
      }
    },

    computed: mapState({
      message: state => state.notification.message,
      pageTitle: state => state.appheader.title,
      transparent: state => state.appheader.transparent,
      iconColor: state => state.appheader.iconColor,
      backBtn: state => state.appheader.backBtn,
      backBtnPath: state => state.appheader.backBtnPath,
      settingsBtn: state => state.appheader.settingsBtn,
      settingsIcon: state => state.appheader.settingsIcon,
      settingsBtnOnClick: state => state.appheader.settingsBtnOnClick
    }),

    methods: {
      ...mapActions([
        'loadPlants',
        'loadSettings',
        'loadTags',
        'hideNotification'
      ])
    },

    mounted () {
      Promise.all([
        this.loadSettings(),
        this.loadPlants(),
        this.loadTags()
      ])
    },

    updated () {
      if (this.message) {
        setTimeout(this.hideNotification, this.notificationTimeout)
      }
    }
  }
</script>

<style lang="postcss">
  @import "normalize.css";
  @import "../styles/colors";
  @import "../styles/animations";
  @import "../styles/media-queries";
  @import "../styles/fonts";
  @import "../styles/typography";
  @import "../styles/forms";
  @import "../styles/buttons";
  @import "../styles/layout";

  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  body {
    display: flex;
    justify-content: center;
    background: var(--background-secondary);

    &.js-no-scrolling {
      overflow: hidden;
    }
  }

  #app {
    width: 100vw;
    min-height: 100vh;
    height: 100%;
  }

  .notifications {
    z-index: 4;
  }

  .main-wireframe {
    display: flex;
    height: 100%;
    min-height: 100vh;
    flex-direction: column;
    justify-content: flex-start;
    align-items: stretch;
    padding-top: var(--app-header-size);
  }

  .app-content {
    width: 100%;
    max-width: var(--app-mobile-max-size);
    margin: 0 auto;

    @media (--min-desktop-viewport) {
      max-width: var(--app-desktop-max-width);
    }
  }

  .svg-icon {
    display: inline-block;
    width: 16px;
    height: 16px;
    color: inherit;
    vertical-align: middle;
    fill: none;
    stroke: currentColor;

    & path {
      fill: currentColor;
    }
  }

  .svg-fill {
    fill: currentColor;
    stroke: none;
  }
</style>
