---
name: watch4k
description: 用 mpv+Anime4K 看视频/动漫——真·实时超分（最高到 4K 观感），支持 B 站/YouTube 等 yt-dlp 支持的站点
---

# watch4k（真超分播放）

把网页视频交给 mpv 挂 Anime4K 着色器播放：实时卷积超分，动画效果极佳（真人剧提升有限，如实告知用户）。

## 第一步：依赖检测（每次先跑）

```bash
yt-dlp --version && mpv --version
```

缺哪个就给用户安装命令（不要自己装）：
- yt-dlp：`winget install yt-dlp.yt-dlp`
- mpv：`winget install shinchiro.mpv`（装完重开终端才有 PATH）

## 第二步：首次运行准备着色器

```bash
# 检查着色器目录（~/.catagent/skills/watch4k/shaders/ 有 .glsl 文件即跳过）
ls ~/.catagent/skills/watch4k/shaders/ 2>/dev/null | head -3
```

没有就装一次：
```bash
git clone --depth 1 https://github.com/bloc97/Anime4K /tmp/anime4k
mkdir -p ~/.catagent/skills/watch4k/shaders
cp /tmp/anime4k/glsl/Moderate/*.glsl ~/.catagent/skills/watch4k/shaders/ 2>/dev/null || cp /tmp/anime4k/glsl/ModeA*.glsl ~/.catagent/skills/watch4k/shaders/
rm -rf /tmp/anime4k
```

## 第三步：播放（一行命令）

mpv 内置 ytdl_hook——**直接给它网页地址**，不用手动提流：

```bash
mpv --fs --title="watch4k" --glsl-shaders="~/.catagent/skills/watch4k/shaders/Anime4K_Clamp_Highlights.glsl;~/.catagent/skills/watch4k/shaders/Anime4K_Restore_CNN_M.glsl;~/.catagent/skills/watch4k/shaders/Anime4K_Upscale_CNN_x2_M.glsl;~/.catagent/skills/watch4k/shaders/Anime4K_AutoDownscalePre_x2.glsl;~/.catagent/skills/watch4k/shaders/Anime4K_AutoDownscalePre_x4.glsl;~/.catagent/skills/watch4k/shaders/Anime4K_Upscale_CNN_x2_M_S.glsl" "<视频页URL>"
```

注意：
- Windows/Git Bash 下分号分隔的 shader 列表必须整段引号包裹；路径用正斜杠
- 着色器文件名以 shaders/ 目录实际存在的为准（Moderate/Quality 套装文件名可能略异），拼命令前先 `ls` 确认
- `--fs` 全屏；想置顶加 `--ontop`
- 登录墙内容（B 站会员等）：mpv 提不了流时如实告知用户限制，可尝试 `--ytdl-raw-options=cookies-from-browser=chrome`（读浏览器 cookie，需用户同意）

## 输出约定

告诉用户：已用 mpv 打开（Anime4K 超分）；窗口控制快捷键：`空格`暂停、`f` 全屏切换、`q` 退出。
