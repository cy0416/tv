# UZ影视专用接口（基于本仓库自身线路生成）

> 本目录的源**只来自本仓库**（`dc2.txt` 那条线路展开后的站点），**未掺入任何第三方仓库的源**。
> 格式与 TVBox 的 `dc*.txt` 不同，UZ 不能直接吃 `dc2.txt`，所以这里把它转成了 UZ 格式。

## 一、接口地址（UZ 里直接填）

| 用途 | 地址 | 填在哪 |
|---|---|---|
| **订阅包（推荐，一键全导）** | `https://cdn.jsdmirror.com/gh/cy0416/tv@master/tvbox/uz/uzAio.zip` | 设置 → 数据管理 → **订阅** → ➕ → 输入链接 |
| 采集源 | `https://cdn.jsdmirror.com/gh/cy0416/tv@master/tvbox/uz/cms.json` | 设置 → 数据管理 → **视频源** → ⚙️ → 添加源列表 |
| 直播源 | `https://cdn.jsdmirror.com/gh/cy0416/tv@master/tvbox/uz/live.json` | 设置 → 数据管理 → **直播源** → ⚙️ → 添加源列表 |

> 本仓库原有的 TVBox 线路（`dc*.txt`、`dc2.txt` 等）**完全未改动，照旧可用**。

## 二、内容说明

| 项 | 说明 |
|---|---|
| 来源 | `dc2.txt` → 22 个 `cache/*.html` + `thirdparty/*` + `author/*` + `live.json`，共 42 条线路展开 |
| 采集源 | **33 条**（已逐条探测存活；同名/同地址已去重） |
| 直播源 | 1 条：本仓库 `tvbox/live.txt`（175 条频道） |
| 过滤 | TVBox 的 `csp_*/jar` 类站点（2349 个）UZ **不支持**，已剔除；成人源 10 条已隔离到 `cms_adult.json`，默认不进订阅包 |

## 三、为什么 TVBox 的源不能直接给 UZ

| | TVBox / 影视仓 | UZ影视 |
|---|---|---|
| 线路文件 | `{"urls":[{"url":"...html"}]}` | ❌ 不认 |
| 站点 | `sites[]`（type=1/3，含 jar/drpy） | 只吃采集源 `{api,name}` 与 UZ 自有 JS 扩展 |
| 直播 | `lives[]`（含 proxy base64） | `[{name,api,m3u,type:10}]` |

**唯一两边通用的就是「采集源(CMS)」**，所以本目录只转这一部分。

## 四、更新方式

源会失效，重新生成即可（会重新探测存活并覆盖本目录）：

```bash
python build_uz.py --check      # 生成 + 存活检测
python upload_uz.py             # 上传到仓库 tvbox/uz/
```

- `build_uz.py`：从本仓库 `dc2.txt` 抓取 → 解析 → 去重 → 探测 → 生成 `out/`
- 生成物：`cms.json`、`cms_adult.json`、`live.json`、`uzAio.zip`

## 五、TVBox 侧：直播源格式已修正（2026-10-13）

仓库根部的 `tvbox/live.json` 原先用的是老式 `proxy://` 内嵌写法
（`{"lives":[{"group","channels":[{"name","urls":["proxy://…"]}]}]}`），
FongMi / 影视仓 / OK影视 不识别，导致多仓里那条直播线路被当成点播源显示、加载不出频道。

现已改为现代标准格式（`{name,type:0,url,playerType,epg}`），指向仓库自己的 `tvbox/live.txt`（175 频道）：

```jsonc
// tvbox/live.json
{ "lives": [ { "name": "📺我的直播", "type": 0,
  "url": "https://cdn.jsdmirror.com/gh/cy0416/tv@master/tvbox/live.txt",
  "playerType": 1, "epg": "https://epg.112114.xyz/?ch={name}&date={date}" } ] }
```

- 多仓线路照旧可用；也建议在「设置 → 直播源」把这**同一条 URL** 加成独立直播订阅，切线路也不影响直播；
- 相关脚本：`build_live.py`（生成）、`lock_cfg.py`（2423 加密）、`upload_file.py`（单文件上传）。

## 六、格式备忘

```jsonc
// cms.json
[ { "api": "https://xxx.com/api.php/provide/vod/", "name": "🔵源名" } ]

// live.json
[ { "name": "📡我的直播源", "api": "https://.../live.txt", "type": 10, "remark": "备注" } ]
```
