<template>
  <Window :title="title">
    <div class="wrap">
      <!-- <Tabs> -->
        <!-- <Tab label="BASIC" active> -->
          <div class="form-item">
            <label>드라이브 명</label>
            <input type="text" autofocus placeholder="ex) 지오맥 드라이브" disabled v-model="conn.name">
          </div>

          <h1 class="section-title">로그인 정보</h1>
          <div class="form-row">
            <!-- <div class="form-item">
              <label>IP/Host</label>
              <input type="text" placeholder="eg. 127.0.0.1 or my.domain.com" v-model="conn.host">
            </div> -->
            <!-- <div class="form-item" style="flex: 0 0 80px"> -->
              <!-- <label>Port</label> -->
              <!-- <input type="text" placeholder="eg. 22" v-model.number="conn.port"> -->
            <!-- </div> -->
          </div>
          <div class="form-item" style="flex: 0 0 80px">
            <label>아이디</label>
            <input type="text" placeholder="" v-model="conn.user">
          </div>
          <!-- <div class="form-item"> -->
            <!-- <label>Authentication method</label> -->
            <!-- <select v-model="conn.authType" @change="authTypeChange"> -->
              <!-- <option value="password">Password</option> -->
              <!-- <option value="password-ask">Password (ask on connect)</option> -->
              <!-- <option value="key-file">Private Key (file)</option> -->
              <!-- <option value="key-input" disabled>Private Key (input)</option> -->
            <!-- </select> -->
          <!-- </div> -->
          <div v-show="conn.authType === 'password'" class="form-item">
            <label>패스워드</label>
            <input type="password" v-model="conn.password">
          </div>
          <div v-show="conn.authType === 'key-file'" class="form-item">
            <label>Key File</label>
            <input type="text" placeholder="eg. C:\Users\me\.ssh\id_rsa" v-model="conn.keyFile">
          </div>
          <div v-show="conn.authType === 'key-input'" class="form-item">
            <label>Key</label>
            <textarea placeholder="eg. ssh-rsa AAAAB3Nz..." v-model="conn.key"></textarea>
          </div>

          <!-- <h1 class="section-title">Remote</h1>
          <div class="form-item">
            <label>Path</label>
            <input type="text" placeholder="eg. /home/john" v-model="conn.folder">
          </div> -->

          <!-- <h1 class="section-title">Local</h1>
          <div class="form-item">
            <label>Drive letter</label>
            <select v-model="conn.mountPoint">
              <option v-for="drive in drives" :value="drive + ':'" :key="drive">{{drive}}:</option>
            </select>
          </div> -->
        <!-- </Tab> -->
        <!-- <Tab label="ADVANCED">
          <div class="form-item">
            <SwitchLabel label="Connect on Startup" v-model="conn.advanced.connectOnStartup"/>
          </div>

          <div class="form-item">
            <SwitchLabel label="Custom Command Line params" v-model="conn.advanced.customCmdlOptionsEnabled"/>

            <CustomCmdlOptions v-model="conn.advanced.customCmdlOptions"/>
          </div>
        </Tab> -->
      <!-- </Tabs> -->

      <div class="footer">
        <button class="btn" @click="cancel">종료</button>
        <button class="btn default" @click="save">로그인</button>
      </div>
    </div>
  </Window>
</template>

<script>
import { remote } from 'electron'
import { v4 as uuid } from 'uuid'

import Window from '@/components/Window'
import Tabs from '@/components/Tabs/Tabs'
import Tab from '@/components/Tabs/Tab'
import SwitchLabel from '@/components/SwitchLabel'
import CustomCmdlOptions from './CustomCmdlOptions'

const windowManager = remote.require('electron-window-manager')

export default {
  name: 'add-edit-connection-window',

  components: {
    Window,
    Tabs,
    Tab,
    SwitchLabel,
    CustomCmdlOptions
  },

  methods: {
    cancel () {
      windowManager.closeCurrent()
    },

    save () {
      this.conn.advanced.customCmdlOptions =
        this.conn.advanced.customCmdlOptions.filter(a => a.name !== '')

      if (this.isEditingMode) {
        this.$store.dispatch('UPDATE_CONNECTION', this.conn)
      } else {
        this.$store.dispatch('ADD_CONNECTION', this.conn)
      }

      windowManager.closeCurrent()
    },

    authTypeChange () {
      this.conn.password = ''
      this.conn.keyFile = process.env.USERPROFILE + '\\.ssh\\id_rsa'
    }
  },

  data () {
    return {
      isEditingMode: false,

      title: 'Geomec Cloud Manager 로그인',
      drives: 'EFGHIJKLMNOPQRSTUVWXYZ',

      conn: {
        uuid: uuid(),
        name: 'Geomec Cloud',
        host: 'cloud.stpsystem.co.kr',
        port: 22,
        user: '',
        folder: '/Data Storage/File Server',
        authType: 'password',
        password: '',
        keyFile: process.env.USERPROFILE + '\\.ssh\\id_rsa',
        key: '',
        mountPoint: 'E:',
        status: 'disconnected',
        pid: 0,
        advanced: {
          customCmdlOptionsEnabled: false,
          customCmdlOptions: [],
          connectOnStartup: false
        }
      }
    }
  },

  mounted () {
    if (this.$route.name === 'edit-connection') {
      this.isEditingMode = true

      this.title = '수정 모드'

      this.conn = this.$store.state.Data.connections.find(a => a.uuid === this.$route.params.uuid)
    }
  }
}
</script>

<style lang="less" scoped>
.wrap .footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 15px;
  text-align: right;
}
</style>
