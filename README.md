# dailyleaf-support

> 一日笺 OneDayLeaf 的**隐私政策**与**技术支持**中心 —— 中/英/日/韩四语静态站点，通过 GitHub Pages 发布。
>
> - 线上首页：https://cosmopolition.github.io/dailyleaf-support/
> - 隐私政策：https://cosmopolition.github.io/dailyleaf-support/privacy.html
> - 技术支持：https://cosmopolition.github.io/dailyleaf-support/support.html
>
> 根目录的 `index.html` / `privacy.html` / `support.html` 会按访问者的浏览器语言自动分流到对应语言目录。

---

# 一日笺 OneDayLeaf Support

一日笺(OneDayLeaf · iOS App)的隐私政策与技术支持中心,中/英/日/韩四语,作为独立静态站点部署到 GitHub Pages。

## 为什么单独一个仓库

商店页要填两个 URL(App Store Connect 的 **Privacy Policy URL** 和 **Support URL**),
但 App 本身**不联网** ⇒ 这两个地址不能由 App 自己的服务器提供 ⇒ 放在一个公开的静态仓库里,
用 GitHub Pages 托管,零成本、零运维、不会因为 App 下架而失效。

## 页面结构

```
support/
├── index.html          # 根重定向页(按浏览器语言 → ja/ ko/ zh/ en/)
├── privacy.html        # 隐私政策重定向页(根路径,直接填进 App Store Connect 也能落到对应语言)
├── support.html        # 帮助中心重定向页
├── css/
│   └── style.css       # 共享浅色主题(纸纹 + 暖金 accent + 朱砂印,与 App Palette 对齐)
├── en/
│   ├── privacy.html    # 隐私政策(英文)
│   └── support.html    # 帮助中心(英文)
├── ja/
│   ├── privacy.html    # 隐私政策(日文)
│   └── support.html    # 帮助中心(日文)
├── ko/
│   ├── privacy.html    # 隐私政策(韩文)
│   └── support.html    # 帮助中心(韩文)
└── zh/
    ├── privacy.html    # 隐私政策(中文)
    └── support.html    # 帮助中心(中文)
```

## 页面内容

### 隐私政策(`/privacy.html`)

13 个条款:我们收集什么 / 权限 / 数据存在哪 / 网络 / 第三方 / iCloud 备份 / 导出与分享 /
数据安全 / 儿童隐私 / 你的权利 / App Store 隐私标签对照 / 政策变更 / 联系我们。

内容**基于 App 的真实技术边界**,不是套模板 —— 每一条都能对着代码核对:

- **零权限** —— `Info.plist` 里只有 `NSPhotoLibraryAddUsageDescription`(存回相册),
  选图走 `PhotosPicker` 不需要任何权限;相机、麦克风、通讯录、位置一律不申请
- **零网络** —— 源码里没有 `URLSession`、没有 `URLRequest`、没有任何 http 请求代码;
  唯一一次通信是买断时与 App Store 的 StoreKit 握手(付款由 Apple 处理)
- **数据全在本机** —— `Documents/Archive/`(笺集)、`Documents/Archive/.trash/`(回收站)、
  `Documents/Draft/`(草稿)、`UserDefaults`(语言/纸色/落款/解锁标志)
- **照片的 EXIF 被剥掉** —— 落盘时 `jpegData()` 重新编码,拍摄时间、机型、GPS 坐标全丢
- **无服务器** —— 所以「删除 App = 删除全部」,代价是没有云端找回
- **不额外加密** —— 明文,换来「你随时能打开核对」,这一点在政策里明说

### 帮助中心(`/support.html`)

应用介绍 + 4 个功能卡 + **14 条 FAQ** + 联系邮箱。

FAQ 以隐私向问题为主(存到相册要不要授权 / 照片会不会上传 / 换手机怎么办 / 备份里有什么 /
从备份恢复会不会覆盖 / 怎么彻底删数据),因为这是用户最先想确认的事。

## 部署

### GitHub Pages

1. 新建一个**公开**仓库(建议名 `dailyleaf-support`,与 `eyecare-support` 同款命名)
2. 把本目录 `support/` 里的**内容**(不是 `support/` 这一层)推到仓库根:
   ```bash
   cd <仓库目录>
   cp -R /Users/moneywang/Desktop/dailyleaf/support/. .
   git add -A && git commit -m "OneDayLeaf 支持中心" && git push
   ```
3. 仓库 **Settings → Pages** → Source 选 `main` 分支、`/ (root)` 目录 → Save
4. 站点上线:`https://cosmopolition.github.io/dailyleaf-support/`

### 填进 App Store Connect

| 字段 | 填什么 |
|---|---|
| Privacy Policy URL | `https://cosmopolition.github.io/dailyleaf-support/privacy.html` |
| Support URL | `https://cosmopolition.github.io/dailyleaf-support/support.html` |

根路径那两个页面会按访问者的浏览器语言自动跳到对应语言,所以**直接填根路径的完整地址即可**,
不必为四种语言各建一条 URL。

### 描述正文末尾

四语商店描述的最后都附了这两个地址(真源 `store/metadata/store-metadata.json` 的 `description` 字段):

```
隐私政策:https://cosmopolition.github.io/dailyleaf-support/privacy.html
技术支持:https://cosmopolition.github.io/dailyleaf-support/support.html
```

> ⚠️ **仓库名或组织名不是 `cosmopolition/dailyleaf-support` 时**,要改这三处:
> ① `store/metadata/store-metadata.json` 四语 `description` 末尾的四个地址
> ② 本 README 里的地址
> ③ App Store Connect 的两个 URL 字段
> 改完跑 `python3 tools/store_meta.py gen && python3 tools/store_meta.py check`。

### 自定义域名(可选)

仓库根目录加 `CNAME` 文件写上域名,再到域名服务商配 CNAME 记录指向 `<org>.github.io`。

### 本地预览

```bash
cd support
python3 -m http.server 8000
# 浏览器访问 http://localhost:8000
```

## 技术细节

- 纯 HTML + CSS,无框架、无构建步骤、无 JS(根重定向页那点 `navigator.language` 分流除外)
- 浅色纸纹主题,背景叠了一层 `feTurbulence` 噪点(opacity 0.035)
- CSS 变量统一管理主题色彩,取自 `RenderCoreRunner/OneDayLeafRunner/Theme.swift` 的 Palette:
  - `--paper: #FBFAF7`、`--ink: #1F1F1F`、`--ink-mid: #5F5A51`、`--ink-light: #736E64`
  - `--line: #DCD8CE`、`--accent: #C6A87C`(暖金)、`--seal: #C0382C`(朱砂印)
  - App 是浅色的,站点也浅色:用户从 App 里点开链接,看到的是同一个视觉体系
- `<details>` / `<summary>` 实现 FAQ 手风琴交互
- 字体栈已含 `PingFang SC` / `Noto Sans CJK SC` / `Hiragino Sans` / `Yu Gothic` /
  `Apple SD Gothic Neo` / `Malgun Gothic`,中日韩文都能正常渲染
- logo 的朱砂方印用中文「笺」字,与 App 里印在笺上的那枚 seal 同色

## 语言切换

页面右上角有语言切换,顺序统一为 **EN | 中文 | 日本語 | 한국어**,当前页高亮。
根目录的三个重定向页按浏览器语言分流:`ja*` → `ja/`、`ko*` → `ko/`、`zh*` → `zh/`、其余 → `en/`。

新增语言时:`xx/privacy.html`、`xx/support.html` 两份,照抄现有一页的结构与类名即可
(结构必须与中文基准 1:1 —— 13 条隐私条款 / 14 条 FAQ)。

## 联系

Email: peer2peer.wang@gmail.com
