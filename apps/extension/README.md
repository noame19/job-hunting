# @job-hunting/extension

## Browsers support

| [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png" alt="Edge" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)<br/> Edge | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png" alt="Chrome" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)<br/>Chrome | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png" alt="Firefox" width="24px" height="24px" />](https://www.mozilla.org/firefox/)<br/>Firefox / Zen |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Edge                                                         | last version                                                 | Firefox 115 ESR 或更新版本 / Zen Browser（同 Firefox Gecko 内核） |

### Firefox / Zen Browser 兼容性说明

本项目使用 WXT 构建，支持生成 MV3 Firefox 扩展（产物在 `.output/firefox-mv3/` 与 `.output/job-hunting-extension-<version>-firefox.zip`）。Firefox 内核浏览器（含 Zen Browser）因为不实现 Chrome 的 `chrome.offscreen` API，后台脚本会跳过 Offscreen Document，直接以模块 Worker 形式启动数据库服务并桥接消息；Manifest 会自动移除 `offscreen`、`declarativeNetRequestWithHostAccess` 等 Chrome 专属权限并补上 `browser_specific_settings.gecko.id`。

构建/调试命令：

```bash
# 安装依赖
pnpm i

# Firefox 开发模式（热更新，需先安装 web-ext）
pnpm --filter @job-hunting/extension run dev:firefox

# Firefox 生产构建
pnpm --filter @job-hunting/extension run build:firefox

# 打包 zip 用于 about:debugging 临时加载或提交 AMO
pnpm --filter @job-hunting/extension run zip:firefox
```

在 Zen Browser / Firefox 中加载：

1. 打开 `about:debugging#/runtime/this-firefox`
2. 选择 “临时载入附加组件”，选择 `.output/firefox-mv3/manifest.json`，或直接选择 `.output/job-hunting-extension-<version>-firefox.zip`
3. 若想正式分发，需要把 zip 提交到 [addons.mozilla.org](https://addons.mozilla.org/) 审核并签名

已知限制（上游 issue [#2](https://github.com/lastsunday/job-hunting/issues/2)）：

- Firefox 上 PGlite + OPFS 的持久化路径在 Gecko 上存在性能与稳定性问题（建表 10s+、偶发 `ERRORDATA_STACK_SIZE exceeded`），本分支将 Firefox 默认切换为 PGlite 内置的 IndexedDB 后端（`idb://job-hunting-pgdata`），不再走 OPFS-AHP，从而规避上述崩溃。Chrome / Edge 继续走原来的 `opfs-ahp://` 路径，保持行为零回归。



## 运行及编译

**编译**

1. 安装，编译

```bash
    pnpm i
    pnpm run build
```

1. 打开 chrome，选择加载已解压的扩展程序，选择当前项目的 .output/chrome-mv3 目录

2. 打开页面
   - boss 直聘： <https://www.zhipin.com/web/geek/jobs>
   - 51Job： <https://we.51job.com/pc/search>
   - 智联招聘： <https://www.zhaopin.com/jobs>、<https://www.zhaopin.com/sou>（可能重定向到 jobs）
   - 拉钩网：<https://www.lagou.com/wn/zhaopin>
   - 猎聘网： <https://www.liepin.com/zhaopin>
   - 就业在线： <https://www.jobonline.cn/position>
   - 广东公共求职招聘服务平台 <https://ggfw.hrss.gd.gov.cn/recruitment/internet/main/#/search?type=1>

**开发**

1. 安装，编译

   ```bash
   pnpm i
   pnpm run dev
   ```

2. chrome 浏览器打开 chrome://extensions/ 页面

3. 点击`加载已解压的扩展程序`

4. 选择项目中生成的 .output/chrome-mv3-dev 文件夹即可

5. 每次保存都会重新编译，扩展程序需要**_重新点一次刷新按钮_**才生效

## 测试

> <https://vitalets.github.io/playwright-bdd/>

## Thanks

1. <https://github.com/tangzhiyao/boss-show-time> **_boss 直聘时间展示插件_**
2. <https://github.com/iibeibei/tampermonkey_scripts> **_BOSS 直聘 跨境黑名单_**
3. <https://kjxb.org/> **_【跨境小白网】，跨境电商人的职场交流社区，互助网站。_**
4. <https://maimai.cn/article/detail?fid=1662335089&efid=I0IjMo8A_37C2pHoqU2HjA> **_求职必备技能：教你如何扒了公司的底裤_**
5. <https://github.com/it-job-blacklist/996ICU.job.blacklist_company> **_主要城市996公司名单，互联网企业黑名单，找工作防止掉坑_**
6. <http://www.blackdir.com> **_IT黑名单_**
7. <https://www.reshot.com> **Free Icons & IllustrationsDesign freely with instant downloads and commercial licenses.**
8. <https://github.com/wxt-dev/wxt> **Next-gen Web Extension Framework**
9. <https://github.com/ant-design/ant-design> **An enterprise-class UI design language and React UI library**
