<template>
  <div id="application-bar" flat small>
    <v-layout align-center row
      fill-height :class="`${getPlatform() === 'darwin' ? 'space-left' : null}`">
      <p v-if="isLoggedIn" class="app-title">
        DBFS-Explorer
      </p>
      <v-btn v-if="isLoggedIn()"
        v-for="item in buttons.left" small
        :key="item.id" @click="item.callback"
        icon light class="drag-safe btn">
        <v-icon small>{{ item.icon }}</v-icon>
      </v-btn>
      <v-spacer />
      <v-btn
        v-for="item in buttons.right" small
        :key="item.id" @click="item.callback"
        v-if="(item.platforms.findIndex(x => x === getPlatform()) > -1)"
        icon light class="drag-safe btn">
        <v-icon small>{{ item.icon }}</v-icon>
      </v-btn>
    </v-layout>
  </div>
</template>

<script>
export default {
  name: 'application-bar',
  props: {
    getPlatform: {
      type: Function,
      required: true
    },
    isLoggedIn: {
      type: Function,
      required: true
    }
  },
  data () {
    return {
      buttons: {
        left: [
          { id: 'id-app-connect', text: 'Connect', icon: 'fa-cloud', callback: this.connect, platforms: ['darwin', 'win32', 'linux'] },
          // { id: 'id-app-download', text: 'Download', icon: 'fa-download', callback: () => {}, platforms: ['darwin', 'win32', 'linux'] },
          { id: 'id-app-delete', text: 'Delete', icon: 'fa-trash', callback: () => {}, platforms: ['darwin', 'win32', 'linux'] }
        ],
        right: [
          { id: 'id-app-about', text: 'About', icon: 'fa-star', callback: () => {}, platforms: ['darwin', 'win32', 'linux'] },
          { id: 'id-app-logout', text: 'Logout', icon: 'fa-power-off', callback: () => { console.log(this.isLoggedIn()) }, platforms: ['darwin', 'win32', 'linux'] },
          { id: 'id-app-maximize', text: 'Maximize', icon: 'fa-window-maximize', callback: this.maximizeApp, platforms: ['win32', 'linux'] },
          { id: 'id-app-minimize', text: 'Minimize', icon: 'fa-minus', callback: this.minimizeApp, platforms: ['win32', 'linux'] },
          { id: 'id-app-close', text: 'Close', icon: 'fa-times', callback: this.closeApp, platforms: ['win32', 'linux'] }
        ]
      }
    }
  },
  methods: {
    connect: function () {
      console.log('on Click Connect')
    },
    maximizeApp: function () {
      this.$electron.remote.getCurrentWindow().maximize()
    },
    minimizeApp: function () {
      this.$electron.remote.getCurrentWindow().minimize()
    },
    closeApp: function () {
      this.$electron.remote.getCurrentWindow().close()
    }
  }
}
</script>