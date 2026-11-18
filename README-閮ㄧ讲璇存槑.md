# MedAxis PWA — 部署与安装说明

这是一套可直接部署的 PWA（可安装网页 App）。部署到任意 HTTPS 静态托管后，手机浏览器打开
即可「添加到主屏幕」，从图标启动全屏运行、无地址栏，观感与原生 App 一致，且支持离线打开。

## 目录内容

| 文件 | 作用 |
|---|---|
| `index.html` | 你的 App（在原文件基础上注入了 manifest / Apple meta / SW 注册） |
| `manifest.webmanifest` | App 清单：名称、图标、全屏模式、主题色 |
| `sw.js` | Service Worker：缓存优先，离线可用 |
| `icon-192.png` / `icon-512.png` | 主图标 |
| `icon-maskable-512.png` | 自适应图标（Android 圆形/方圆形裁切安全区） |
| `apple-touch-icon.png` | iOS 主屏图标 |
| `favicon-64.png` | 浏览器标签图标 |

## 一键部署（任选其一，都是免费 HTTPS）

**方式 A — Vercel（最省事）**
1. 把整个 `medaxis-pwa` 文件夹传到 GitHub 仓库
2. 打开 vercel.com → New Project → 导入该仓库 → Deploy
3. 得到 `https://xxx.vercel.app`，手机打开即可

**方式 B — Netlify Drop（不用注册仓库）**
1. 打开 app.netlify.com/drop
2. 把 `medaxis-pwa` 文件夹直接拖进去
3. 秒得一个 `https://xxx.netlify.app` 链接

**方式 C —  GitHub Pages**
把文件夹推到仓库 → Settings → Pages → 选 main 分支根目录 → 得到 `https://用户名.github.io/仓库名/`

> 注意：PWA 的 Service Worker 与「安装」能力要求 **HTTPS**（`localhost` 例外）。
> 直接双击本地文件（`file://`）打开不会触发安装，必须走 https 链接。

## 手机上安装

- **Android（Chrome）**：打开链接 → 地址栏会出现「安装」图标，或浏览器菜单 →「安装应用 / 添加到主屏幕」
- **iPhone（Safari）**：打开链接 → 底部分享按钮 →「添加到主屏幕」→ 确认

装好后从主屏图标启动，就是全屏 App 体验，断网也能打开。

## 生成扫码用的二维码

部署拿到 https 链接后，用任意二维码工具生成即可，例如：

```bash
# 需要 pip install qrcode[pil]
python3 -c "import qrcode; qrcode.make('https://你的链接/').save('qr.png')"
```

把 `qr.png` 发给客户/朋友，扫码即装。

## 想改的东西

- App 名称：改 `manifest.webmanifest` 里的 `name` / `short_name`
- 主题色（状态栏颜色）：改 `manifest.webmanifest` 的 `theme_color` 和 `index.html` 里的 `theme-color`
- 图标：替换 `icon-*.png` / `apple-touch-icon.png`（建议各尺寸都给一份）
- 更新缓存：改动内容后，把 `sw.js` 里的 `CACHE = "medaxis-v1"` 版本号 +1，用户下次打开会自动更新
