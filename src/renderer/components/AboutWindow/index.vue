<template>
  <Window title="About">
    <div class="wrap">
      <div class="logo-container">
        <img class="logo" src="@/assets/app-logo.png">
      </div>
      <div class="content">
        <h1>Geomec-Cloud Manager</h1>
        <p class="repo-url">
          <a href="https://github.com/writingdeveloper">github.com/writingdeveloper</a>
        </p>
        <!-- <p class="donate-btn">
          <a href="https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=HXZUJ8WX47238">
            <img src="https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif" alt="Donate" title="PayPal - The safer, easier way to pay online!">
          </a>
        </p> -->
        <p>
          Version: {{appVersion}}<br>
          개발자: Geomec 개발자 이시형 (<a href="https://github.com/writingdeveloper/Geomec-Storage-Manager">@writingdeveloper</a>)
        </p>
        <p>이 프로그램은 <a href="https://opensource.org/licenses/MIT">MIT license</a> 라이센스를 따르고 있습니다.</p>
        <p>이 프로그램은 아래와 같은 오픈소스 프로젝트들을 포함하고 있습니다.
          <ul>
            <li>Node.js (<a href="https://github.com/nodejs/node">https://github.com/nodejs/node</a>)</li>
            <li>Electron (<a href="https://github.com/electron/electron">https://github.com/electron/electron</a>)</li>
            <li>Vue.js (<a href="https://github.com/vuejs/vue">https://github.com/vuejs/vue</a>)</li>
            <li>Electron-Vue (<a href="https://github.com/SimulatedGREG/electron-vue">https://github.com/SimulatedGREG/electron-vue</a>)</li>
            <li>SSHFS-Win (<a href="https://github.com/billziss-gh/sshfs-win">https://github.com/billziss-gh/sshfs-win</a>)</li>
          </ul>
        </p>
        <p>버튼 아이콘들은 <a href="https://icons8.com">Icons8.com</a>에서 제공되었습니다.</p>
      </div>
    </div>
  </Window>
</template>

<script>
import { shell, remote } from 'electron'

import Window from '@/components/Window'

const app = remote.app

export default {
  name: 'about-window',

  components: {
    Window
  },

  data () {
    return {
      appVersion: app.getVersion()
    }
  },

  mounted () {
    this.$el.querySelectorAll('a').forEach(item => {
      item.addEventListener('click', event => {
        event.preventDefault()

        shell.openExternal(item.href)
      })
    })
  }
}
</script>

<style lang="less" scoped>
.wrap {
  color: contrast(@main-color);

  > div {
    display: inline-block;
  }

  .logo-container {
    width: 70px;

    .logo {
      width: 60px;
      position: absolute;
      top: 50px;
    }
  }

  .content {  
    h1 {
      color: @primary-color;
      font-size: 18pt;
      margin-bottom: 5px;
    }

    p {
      font-size: 10pt;
      color: fade(contrast(@main-color), 80%);
      margin: 10px 0;

      &.donate-btn {
        position: fixed;
        top: 40px;
        right: 20px;
      }

      &.repo-url {
        margin-bottom: 20px;
      }
    }

    ul {
      margin-left: 25px;
    }

    a {
      color: lighten(@primary-color, 20%);
      text-decoration: none;

      &:hover {
        text-decoration: underline;
      }
    }
  }
}
</style>
