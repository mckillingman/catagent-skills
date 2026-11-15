---
name: where-to-watch
description: 查一部动漫/电视剧/电影在哪些平台能看——输出 平台|链接|免费/会员|清晰度 对照表
---

# where-to-watch（跨平台搜片）

用户给出片名，查它在哪些流媒体平台有、哪些免费哪些要会员。

## 流程

### 1. 通用搜索打底

```
web_search("<片名> 全集 在线观看 平台 哪个app")
web_search("<片名> 独家 版权 爱奇艺 OR 腾讯视频 OR 优酷 OR bilibili")
```

### 2. 平台定点确认（逐个查，有则确认无则排除）

**B 站（有开放搜索 API，首选）**：
```
web_fetch("https://api.bilibili.com/x/web-interface/search/type?search_type=video&keyword=<URL编码片名>")
```
返回 JSON 里有结果即说明有资源；番剧用 `search_type=media_bangumi`。

**爱奇艺/腾讯视频/优酷/芒果**（JS 渲染，用内嵌浏览器）：
```
browser_navigate("https://so.iqiyi.com/qs/<片名>")
browser_read()   # 提取结果标题，判断有无正片/是否 VIP
```
腾讯：`https://v.qq.com/x/search/?q=<片名>`；优酷：`https://so.youku.com/search_q_<片名>`；芒果：`https://so.mgtv.com/so/k-<片名>`
（注意：这些站点的搜索 URL 结构可能变化，browser_read 读到的内容不对时先 browser_state 确认，再用站内搜索框 browser_type 输入片名）

### 3. 判断要点

- 标题带「VIP」「会员」「用券」→ 会员专享
- 只有预告/切条/解说 → 该平台无正片
- 独家剧集会在通用搜索结果里明确写「XX 独播」

## 输出格式

| 平台 | 链接 | 免费/会员 | 备注 |
|---|---|---|---|
| bilibili | https://... | 免费广告 | 1080P |
| 爱奇艺 | https://... | VIP | 4K 杜比 |

附一句推荐（如「免费首选 B 站」）。

## 规则

- 查不到就说查不到，不编造平台链接
- 链接必须是确认有正片的页面，不是搜索结果页
- 用户想直接看 → 建议 watch4k 技能（mpv 超分播放）
