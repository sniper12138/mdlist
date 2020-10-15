<template>
  <div class="home">
    <div class="layout">
      <Layout style="height: 100vh">
        <Header>
          <Menu mode="horizontal" theme="dark" active-name="1">
            <MenuItem name="1">
              <h1>前端工作规范</h1>
            </MenuItem>
          </Menu>
        </Header>
        <Layout style="height: calc(100vh - 64px)">
          <Sider hide-trigger :style="{ background: '#fff' }">
            <Menu
              :active-name="cur"
              theme="light"
              width="auto"
              @on-select="selectMenu"
            >
              <MenuItem name="0">团队规范指南</MenuItem>
              <MenuItem name="1">全局规范</MenuItem>
              <MenuItem name="2">GIT规范以及使用</MenuItem>
              <MenuItem name="3">合作对接规范</MenuItem>
              <MenuItem name="4">JS代码规范</MenuItem>
              <MenuItem name="5">PC端HTML/CSS规范</MenuItem>
              <MenuItem name="6">移动端规范</MenuItem>
              <MenuItem name="7">Web前端国际化解决方案</MenuItem>
              <MenuItem name="8">字体转换规范</MenuItem>
              <MenuItem name="9">整站变灰（特殊节日规范）</MenuItem>
              <MenuItem name="10">图片压缩规范</MenuItem>
            </Menu>
          </Sider>
          <Layout :style="{ padding: '0 24px 24px' }">
            <Content
              :style="{
                padding: '24px',
                minHeight: '280px',
                background: '#fff',
                overflow: 'auto',
              }"
            >
              <mavon-editor
                class="md"
                v-html="selMd"
                :subfield="false"
                :toolbarsFlag="false"
                :boxShadow="false"
                :ishljs="true"
              />
            </Content>
          </Layout>
        </Layout>
      </Layout>
    </div>
  </div>
</template>

<script>
// import VueMarkdown from "vue-markdown";
import marked from "marked";
import md1 from "@/assets/md/团队规范指南.md";
import md2 from "@/assets/md/全局规范.md";
import md3 from "@/assets/md/GIT规范以及使用.md";
import md4 from "@/assets/md/合作对接规范.md";
import md5 from "@/assets/md/js代码规范.md";
import md6 from "@/assets/md/PC端HTML_CSS规范.md";
import md7 from "@/assets/md/移动端规范.md";
import md8 from "@/assets/md/Web前端国际化解决方案.md";
import md9 from "@/assets/md/字体转换规范.md";
import md10 from "@/assets/md/整站变灰（特殊节日规范）.md";
import md11 from "@/assets/md/图片压缩规范.md";
export default {
  name: "Home",
  // components: {
  //   VueMarkdown,
  // },
  data() {
    return {
      list: [md1, md2, md3, md4, md5, md6, md7, md8, md9, md10, md11],
      cur: "0",
      selMd: null,
    };
  },
  inject: ["routerRefresh"],
  watch: {
    $route(to, from) {
      //监听路由是否变化
      if (to.params.id != from.params.id) {
        let id = to.params.id;
        this.cur = id;
        if (this.list[Number(this.cur)] == undefined) {
          this.$router.push({
            path: "Home/0",
          });
        } else {
          this.selMd = marked(this.list[Number(this.cur)]);
        }
        document.getElementsByClassName("ivu-layout-content")[0].scrollTop = 0;
      }
    },
  },
  mounted() {
    let id = "";
    if (this.$route.params.id) {
      id = this.$route.params.id;
    } else {
      this.$router.push({
        path: "Home/0",
      });
      id = "0";
    }

    this.cur = id;
    if (this.list[Number(this.cur)] == undefined) {
      this.$router.push({
        path: "Home/0",
      });
    } else {
      this.selMd = marked(this.list[Number(this.cur)]);
    }
    this.$nextTick(() => {
      L2Dwidget.init({
        pluginRootPath: "live2dw/", //资源root路径
        pluginJsPath: "lib/", //js相对root的路径
        pluginModelPath: "assets/", //模型相对root的路径
        tagMode: !1,
        debug: !1,
        model: {
          scale: 1,
          jsonPath:
            "https://unpkg.com/live2d-widget-model-haruto@1.0.5/assets/haruto.model.json",
        },
        display: {
          position: "right", //定位
          width: 130, //宽度
          height: 210, //高度
          hOffset: 0, //左右
          vOffset: -40, //上下
        },
        mobile: {
          show: false,
        },
        log: false,
        react: { // 设置透明度
            opacityDefault: 1,
            opacityOnHover: 0.5
        }
      });
    });
  },
  methods: {
    selectMenu(name) {
      this.cur = name;
      this.$router.push({
        path: "/" + this.cur,
      });
      // this.selMd = marked(this.list[Number(this.cur)]);
      // document.getElementsByClassName("ivu-layout-content")[0].scrollTop = 0;
    },
  },
};
</script>
