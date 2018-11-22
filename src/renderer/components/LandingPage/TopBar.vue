<template>
  <div class="topBar">
    <div style="text-align:center;">{{appname}} v{{version}}</div>
    <div id="actionBtn-container">
      <div class="minimize actionBtn" @click="minimize">
        <Tooltip content="最小化" placement="bottom-end" :transfer="true" :delay="500">
          <Icon type="md-remove" color="#fff"></Icon>
        </Tooltip>
      </div>
      <div class="close actionBtn" @click="closeApp">
        <Tooltip content="关闭" placement="bottom-end" :transfer="true" :delay="500">
          <Icon type="md-close" color="#fff"></Icon>
        </Tooltip>
      </div>
    </div>
  </div>
</template>
<script>
  const remote = require('electron').remote
  export default {
    name: 'TopBar',
    computed: {
      version() {
        return this.$store.state.Counter.currentVersion
      },
      appname() {
        return this.$store.state.Counter.appName
      }
    },
    methods: {
      closeApp() {
        try {
          this.$Modal.confirm({
            title: '确认',
            content: '<p>确认关闭并退出本系统？</p>',
            okText: '确认退出',
            onOk: () => {
              remote.app.quit()
            },
            onCancel: () => {
              
            }
          })
        } catch (err) {}
      },
      minimize() {
        remote.BrowserWindow.getFocusedWindow().minimize()
      }
    }
  }
</script>
<style scoped>
  .topBar {
    -webkit-app-region: drag;
    background-color: transparent;
    height: 32px;
    width: 100%;
    padding: 6px;
    -webkit-transition: all .3s;
    transition: all .3s;
    color: rgb(241, 241, 241);
    font-size: 12px;
    line-height: 2em;
    z-index: 11;
    position: relative;
  }

  #actionBtn-container {
    cursor: pointer;
    position: absolute;
    top: 0;
    right: 0;
    position: fixed;
    right: 0;
    top: 0;
    display: flex;
  }

  .actionBtn {
    -webkit-app-region: no-drag;
    font-size: 15px;
    width: 25px;
    height: 25px;
    text-align: center;
    -webkit-transition: all .3s;
    transition: all .3s;
    line-height: 2em;
    font-weight: bold;
    opacity: .6;
  }

  .actionBtn:hover {
    opacity: 1;
  }

  .close {
    margin: 0 8px 0 10px;
  }
</style>