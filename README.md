# 找房雷达 · Findhouse Radar

<p align="center">
  <a href="https://conflux-336.github.io/findhouse-radar/">
    <img src="assets/banner.svg" alt="找房雷达 · 本地优先的租房尽调工作台" width="100%">
  </a>
</p>

<p align="center">
  <a href="https://conflux-336.github.io/findhouse-radar/"><img src="https://img.shields.io/badge/GitHub%20Pages-live-19c8b9?style=flat-square&logo=github&logoColor=white" alt="GitHub Pages"></a>
  <a href="index.html"><img src="https://img.shields.io/badge/single--file-HTML-f5c542?style=flat-square" alt="Single-file HTML"></a>
  <a href="BACKEND.md"><img src="https://img.shields.io/badge/backend-none-f08c63?style=flat-square" alt="No backend"></a>
  <a href="#数据与隐私"><img src="https://img.shields.io/badge/data-local--only-7fd8c9?style=flat-square" alt="Local-first"></a>
  <a href="index.html"><img src="https://img.shields.io/badge/vanilla-JS-f7df1e?style=flat-square&logo=javascript&logoColor=black" alt="Vanilla JS"></a>
  <a href="https://leafletjs.com/"><img src="https://img.shields.io/badge/Leaflet-1.9.4-199900?style=flat-square&logo=leaflet&logoColor=white" alt="Leaflet 1.9.4"></a>
</p>

<p align="center"><b><a href="https://conflux-336.github.io/findhouse-radar/">打开在线版 →</a></b> · 无需安装，数据只存你的浏览器</p>

一个**本地优先**的租房尽调工作台：把你在豆瓣、小红书、闲鱼、贝壳等平台看到的房源手动粘进来，统一打分、去重、比价、算首月成本，并在真实地理位置的热力图上查看分布。

- 纯静态单文件：`index.html`，没有构建步骤，没有后端。
- 数据只存浏览器 `localStorage`，可导出 / 导入 JSON 备份。
- 定位与地图用于**你自己录进来的房源**，不是全网爬虫，也不声称覆盖全网。

> 示例数据全部是虚构的，仅用于预览界面；报告里也会明确标注。

[功能](#功能) · [本地运行](#本地运行推荐) · [数据与隐私](#数据与隐私) · [后端方案](BACKEND.md) · [已知边界](#已知边界)

## 功能

**找房决策**

- 设备定位 + 高德底图，WGS-84 → GCJ-02 校正，热力图按可信度加权。
- 中介味评分：来源、话术、账号近 7 日发帖、是否只发租帖、地铁声称与实测距离。
- 同址严格对比：按小区名或 120m 内坐标归组，租金差 15% / 面积差 10% 以上标红，提示“货不对板”。
- 生活设施半径：地铁、便利店、医院等，优先高德 POI，失败退回 OpenStreetMap。
- 一句话找房：把“4000 以内 一居 朝南 近地铁 不要中介”拆成筛选条件。
- 小区尽调：本机价格带 + 优点 / 风险标签 + 平台搜索入口。
- 行情速览：中位租金、中间 50%、单价、个人直租 / 货不对板数量、小区价格带。
- 尽调报告：一键导出可分享的 HTML，含筛选条件、统计、小区标签和房源清单。

**看房过程**

- 看房计划：已联系 / 已约看 / 已看 / 放弃，按时间排序，过时标红。
- 价格历史：记录每次改价，卡片上显示 `↓¥300`；报告里也带降价标记。
- 首月真实成本：押金、一次付、中介费、水电宽带、搬家添置，算签约现金、每月固定和年化月均。
- 信息完整度：缺面积 / 小区 / 地铁 / 账号时显示「待核实 N」，提醒向房东确认。
- 租房避坑清单：押金、合同、维修、噪音等 8 项，可勾选完成度。
- 收藏、排序、重复合并、JSON 备份。

## 快速开始

### 本地运行（推荐）

浏览器的定位 API 在 `file://` 下经常不可用，建议起一个本地静态服务：

```bash
cd findhouse-radar
python3 -m http.server 8765
# 打开 http://localhost:8765
```

允许定位后点右上角「载入示例」可以先看效果；真实使用请「添加房源」粘贴原帖。

### 在线版

**https://conflux-336.github.io/findhouse-radar/**

已通过 GitHub Pages 发布：`main` 分支根目录、静态托管、无需 Actions。HTTPS 下浏览器才会允许定位；地图瓦片、BigDataCloud 反查和 Overpass 需要联网，但**房源数据不会上传**。

### 部署自己的副本

1. Fork 本仓库，或把 `index.html`、`assets/`、`README.md`、`BACKEND.md`、`.nojekyll` 推到自己的公开仓库。
2. 仓库 **Settings → Pages → Build and deployment**：
   - Source 选 **Deploy from a branch**
   - Branch 选 **main**，目录选 **/ (root)**
3. 等 1–2 分钟，访问 `https://<你的用户名>.github.io/<仓库名>/`。

`.nojekyll` 用来跳过 Jekyll 处理；这个项目没有构建步骤，根目录有 `index.html` 就能直接发布。

## 数据与隐私

- 房源、看房进度、小区尽调、预算口径都存在当前浏览器的 `localStorage`（键名 `findhouse.radar.v1`）。
- 换浏览器、换域名（包括从 `localhost` 换到 GitHub Pages）、清理站点数据，都会看到一份空数据。请用「导出」备份，再用「导入」恢复。
- 「导出 JSON」包含你填写的电话、备注和原帖链接，分享前自己检查一遍。
- 页面会向以下第三方发起请求：
  - 高德地图栅格瓦片（底图）
  - BigDataCloud（把坐标反查成大致地址）
  - Overpass / 高德 Web 服务（周边设施，需自己填高德 Key）
- 没有账号系统、没有埋点、没有自动上传。高德 Key 只存在本机 `localStorage`，不要把它硬编码进源码再公开。

## 已知边界

- **不做自动抓取**：平台登录墙、反爬、同源策略和 ToS 都不适合用纯前端绕过；工具只处理你手动粘贴或录入的内容。
- 中介味是**启发式评分**，不是事实认定；账号画像需要你自己点进主页看「近 7 日发帖」和「是否只发租帖」。
- 热力图只反映你已录入的样本，不代表全网挂牌量或市场均价。
- 地铁距离可以用平台声称值，也可以手动填实测值；没有实测时会明确标出“声称”。
- 周边设施在国内的 OpenStreetMap 覆盖不均，查不到就手动填实测距离。
- 单文件依赖 CDN；离线打开会没有底图和样式。

## 文件

```
findhouse-radar/
├── index.html        # 全部界面、样式、逻辑
├── assets/
│   └── banner.svg    # README 横幅（矢量，无外部依赖）
├── README.md         # 本文件
├── BACKEND.md        # 如果要加后端，需要准备什么
└── .nojekyll         # 告诉 GitHub Pages 不要走 Jekyll
```

## 后续可以做什么

- 多设备同步：需要后端或第三方同步服务，见 [BACKEND.md](BACKEND.md)。
- 自动监控新帖 / 降价：需要定时任务和抓取许可，见 [BACKEND.md](BACKEND.md)。
- 更细的通勤时间：接入路径规划 API，而不只是直线距离。

## 许可

未指定许可证前，默认保留所有权利。要开源请补一个 `LICENSE`（例如 MIT）。
