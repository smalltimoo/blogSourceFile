<template>
  <div id="app">
    <div :class="menuFixed ? 'header header-fixed' : 'header'">
      <div class="menu_bar">
        <div class="menu_bar_list">
          <span
            v-on:click="handleRouter('home')"
            :class="activeMenu === 'home' ? 'active_menu_tags ': 'menu_tags'"
          >Home</span>
          <span
            v-on:click="handleRouter('all')"
            :class="activeMenu === 'all' ? 'active_menu_tags ': 'menu_tags'"
          >Articles</span>
          <span
            v-on:click="handleRouter('tags')"
            :class="activeMenu === 'tags' ? 'active_menu_tags ': 'menu_tags'"
          >Tags</span>
          <span
            v-on:click="handleRouter('resume')"
            :class="activeMenu === 'resume' ? 'active_menu_tags ': 'menu_tags'"
          >Resume</span>
          <span
            v-on:click="handleRouter('gallery')"
            :class="activeMenu === 'gallery' ? 'active_menu_tags ': 'menu_tags'"
          >Photos</span>
          <span
            v-on:click="handleRouter('landing')"
            class="menu_tags landing_tag"
          >Landing</span>
          <span
            v-on:click="jumpBirthday"
            class="menu_tags landing_tag"
          >Birthday</span>
          <a
            href="../static/happy-birstday.html"
            class="menu_tags landing_tag"
          >Birthday2</a>
        </div>
      </div>
    </div>
    <router-view class="router-view" />
    <a
      href="www.smalltimoo.com"
      class="copy-right"
    >©Power by Smalltimoo. All rights reserved.</a>
    <div :class="showCommonFooter ? 'common-footer slide-top':'common-footer hidden'">
      <div class="footer-content">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="210"
          height="50"
        >
          <!-- Created with Method Draw - http://github.com/duopixel/Method-Draw/ -->
          <g>
            <title>background</title>
            <rect
              fill="#000000e0"
              id="canvas_background"
              height="52"
              width="182"
              y="-1"
              x="-1"
            ></rect>
            <g
              display="none"
              overflow="visible"
              y="0"
              x="0"
              height="100%"
              width="100%"
              id="canvasGrid"
            >
              <rect
                fill="url(#gridpattern)"
                stroke-width="0"
                y="0"
                x="0"
                height="100%"
                width="100%"
              ></rect>
            </g>
          </g>
          <g>
            <title>Layer 1</title>
            <text
              stroke="#000000e0"
              transform="matrix(42.95035493119024,0,0,14.26839551373704,-7577.625167709305,-1713.4671859652221) "
              xml:space="preserve"
              text-anchor="start"
              font-family="Helvetica, Arial, sans-serif"
              font-size="24"
              id="svg_1"
              y="142.4375"
              x="177"
              stroke-width="0"
              fill="#000000e0"
            ></text>
            <text
              stroke="#000000e0"
              transform="matrix(1.2861377027320486,0,0,1.2191282994473909,-1.9476415157505471,-0.10169488191604614) "
              font-weight="bold"
              xml:space="preserve"
              text-anchor="start"
              font-family="'Simonetta', serif"
              font-size="30"
              id="svg_2"
              y="32.493545"
              x="9"
              stroke-opacity="null"
              fill="#ffffff"
            >Smalltimoo</text>
          </g>
        </svg>
        <div class="group-content">
          <a
            class="child"
            href="www.dendionk.com"
          >Link 1</a>

          <span id="busuanzi_container_site_pv" class="child" style='display:none'>| 访问量 <span id="busuanzi_value_site_pv"></span> 次 </span>
          <span id="busuanzi_container_site_uv" class="child" style='display:none'>| 访客数 <span id="busuanzi_value_site_uv"></span> 人 </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script scoped>
import { postData } from "./utils/data.ts";
import { getAllCategories } from "./utils/datafilter.ts";
import { value, computed, onMounted } from 'vue-function-api';
require('./utils/love');
export default {
  props: {
    name: "App"
  },
  setup(props, context) {
    let menuFixed = value(false);
    let showCommonFooter = value(true);
    let activeMenu = value("home");
    const jumpBirthday = () => {
      window.open('/happy-birstday.html', '_blank');
    };
    const handleRouter = (dir, categorie = '') => {
      activeMenu = dir;
      let path;
      if (categorie) {
        path = `/${dir}?${dir}=${categorie}`;
      } else {
        path = `/${dir}`;
      }
      context.parent.$router.push(path);
    };
    const menu = () => {
      let scrollCount =
        document.body.scrollTop || document.documentElement.scrollTop;
      if (scrollCount > 130) {
        menuFixed = true;
      } else {
        menuFixed = false;
      }
    };
    // 颜色变化函数
    const changeColor = () => {
      console.info(111);
      // 产生rgb的值
      const r = Math.floor(Math.random() * 255);
      const g = Math.floor(Math.random() * 255);
      const b = Math.floor(Math.random() * 255);
      const rgb = r.toString(16) + g.toString(16) + b.toString(16);
      // toString(radix) 把数字转化为radix（取值范围2~36）进制值表示的字符串
      document.querySelector('#app').style.backgroundColor = "#" + rgb;
      window.setTimeout(changeColor, 2000);
      // setTimeout() 方法用于在指定的毫秒数后调用函数或计算表达式。
    };
    const categories = computed(() => Array.from(new Set(getAllCategories(JSON.parse(postData)))));
    // watch($route, (to, from) => {
    //   showCommonFooter = to.name === "Allpost" || to.name === "Home";
    // });
    onMounted(() => {
      window.addEventListener("scroll", menu);
      document.addEventListener("visibilitychange", () => {
        if (document.visibilityState === 'hidden') {
          document.title = `你干啥坏事去了，咋不理我了呢...`;
        } else {
          document.title = `你又回来了，好开心...`;
          window.setTimeout(() => {
            document.title = `Smalltimoo Home `;
          }, 2000);
        }
      });
      let permission = Notification.permission;
      if (permission === "granted") {
        // 已同意，开始发送通知
        const notice = new Notification("Smalltimoo Home", {
          body: "欢迎你访问 Smalltimoo Home, 祝你玩得开心！🈶\n输入密令可以获得高级权限",
          icon: require("./assets/avatar.png"),
          data: {
            url: "https://www.smalltimoo.com"
          }
        });

        // 点击回调
        notice.onclick = () => {
          window.open(notice.data.url); // 当用户点击通知时，在浏览器打开百度网站
        };
      } else if (permission === "denied") {
        // 不同意，发不了咯
      } else {
        // 其他状态，可以重新发送授权提示
        Notification.requestPermission();
      }
      // window.addEventListener("load", changeColor);
    });
    return {
      menuFixed,
      showCommonFooter,
      activeMenu,
      handleRouter,
      jumpBirthday,
      menu,
      changeColor,
      categories
    };
  },
  watch: {
    $route(to, from) {
      this.showCommonFooter = to.name === "Allpost" || to.name === "Home";
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background: url(http://ww1.sinaimg.cn/large/88b26e1cgy1ftdstdeacbj212f0d73zv.jpg)
    top no-repeat;
  background-size: contain;
  height: 100vh;
}
.avatar {
  height: 3rem;
  width: 3rem;
  background-image: url("./assets/avatar.png");
  background-size: cover;
  display: block;
  border-radius: 50%;
  margin: auto;
}
.el-submenu__title {
  font-family: Arial, Helvetica, sans-serif;
}
.header {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  transition: all 0.5s ease-in-out;
}
.info_title {
  font-size: 12px;
}
.menu_bar {
  margin-top: 2rem;
  margin-bottom: 0.5rem;
}
.menu_bar_list {
  padding: 0px 10px;
  font-size: 1rem;
}
.menu_bar_list .menu_tags {
  text-transform: uppercase;
  padding: 5px 10px;
  cursor: pointer;
  color: #35505b;
  margin: 1rem;
  font-weight: 100;
}
.menu_tags:hover {
  color: #5a8492;
  border-bottom: 1px solid #5a8492;
  padding-right: 2rem;
  transition: all 0.2s ease;
}
.active_menu_tags {
  color: #5a8492;
  border-bottom: 1px solid #5a8492;
  padding-right: 2rem;
  transition: all 0.2s ease;
}
.dropbtn {
  color: #35505b;
  padding: 16px;
  font-size: 16px;
  border: none;
  background: none;
}

.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown-content {
  opacity: 0;
  position: absolute;
  min-width: 100%;
  z-index: 100;
  top: 52px;
}
.header-fixed {
  position: fixed;
  z-index: 1000;
  margin-left: auto;
  margin-right: auto;
  top: 0;
  left: 0;
  right: 0;
  background: #000000ad;
  border-bottom: 1px solid #71717154;
  transition: all 0.5s ease-in-out;
}
.header-fixed .dropdown-content {
  background: #ffffff;
}
.header-fixed .menu_bar {
  margin-top: 0.5rem;
}
.header-fixed .menu_bar_list .menu_tags {
  color: #ffffff;
}
.header-fixed .menu_tags:hover {
  border-bottom-color: #ffffff;
}
.dropdown-content span {
  color: #5a8492;
  padding: 10px 16px;
  text-decoration: none;
  display: block;
  font-weight: 100;
  cursor: pointer;
  font-size: 1.2rem;
  text-align: right;
}
.dropdown-content span:hover {
  color: #35505b;
  border-bottom: 1px solid #5a8492;
  padding-right: 2rem;
  letter-spacing: 1px;
  font-size: 1.3rem;
  transition: all 0.3s ease-out;
}
.post_list {
  text-align: left;
  letter-spacing: 0.5px;
  padding: 10px 4%;
  font-size: 0.9rem;
}

.copy-right {
  margin-top: 4rem;
  padding-right: 2rem;
  height: 2rem;
  display: flex;
  justify-content: flex-end;
  align-items: flex-start;
  cursor: pointer;
  color: rgb(211, 96, 96);
  font-size: 0.9rem;
}
.dropdown:hover .dropdown-content {
  opacity: 1;
  transition: all 0.4s ease-in-out;
}

.dropdown:hover .dropbtn {
  color: #5a8492;
}
@media screen and (max-width: 1000px) and (min-width: 300px) {
  .copy-right {
    display: none;
  }
  .menu_tags:hover {
    color: #5a8492;
    border-bottom: 1px solid #5a8492;
    transition: all 0.2s ease;
    padding-right: 0rem;
  }
  .landing_tag {
    display: none;
  }
}

@media (min-width: 576px) {
  .home_post-list {
    max-width: 540px;
  }
}
@media (min-width: 768px) {
  .home_post-list {
    max-width: 720px;
  }
}
@media (min-width: 992px) {
  .home_post-list {
    max-width: 960px;
  }
}
@media (min-width: 1200px) {
  .home_post-list {
    max-width: 1220px;
  }
}
.common-footer {
  margin-top: 100px;
  background: #000000;
  line-height: 20px;
  font-size: 16px;
  min-height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 100%;
}
.common-footer .footer-content {
  width: 50%;
  display: flex;
  justify-content: space-around;
}
.footer-content svg {
  color: #fff;
  font-weight: 300;
  font-size: 1.4rem;
  cursor: pointer;
  border-top: 1px solid #000000;
  border-bottom: 1px solid #000000;
  transition: all 0.2s ease-in-out;
}
.footer-content svg:hover {
  border-top: 1px solid #ffffff;
  border-bottom: 1px solid #ffffff;
}
.footer-content .group-content {
  min-width: 50%;
  justify-content: space-around;
  display: flex;
  align-items: center;
}
.group-content .child {
  cursor: pointer;
  border-bottom: 1px solid #000000;
  transition: ease-out;
  color: #a5a7a6;
}
.group-content .child:hover {
  border-bottom: 1px solid #42b983;
  color: #42b983;
}
.hidden {
  display: none;
}
/* transform: scale(1.1); */
</style>
