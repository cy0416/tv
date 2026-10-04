# 作者接口备份（来源：ouhaibo1980）

本目录从作者 `ouhaibo1980` 的仓库拷贝的可用接口配置，用于**防删库备份 + 自用**。
拷贝时间：2026-10-04。

## 一、标准 TVBox 接口（来自 tBox，sites 聚合，可直接用）
- `new.json` —— 欧歌网盘 / 豆瓣推荐 / 各网盘 js 源聚合
- `安卓new.json` —— 安卓版聚合接口
- `配置.json` —— 综合配置接口

> 注意：tBox 已停更（最后更新 2024-12），内部站点可能部分失效。

## 二、单站点采集接口（来自 tvbox2/json，每文件 = 一个站点，可独立作线路）
- `bj.json`（北极星类）、`dawo.json`、`ex.json`、`hb.json`、`lb.json`
- `mogg.json`、`og.json`、`sd.json`、`wogg.json`、`xm.json`、`yyds.json`、`zz.json`（至臻等）
> tvbox2 较活跃（2026-09 更新）。

## 三、如何使用
- **作为线路**：把下面任一地址加进 `dc2.txt` 的 urls，或在 TVBox 配置地址直接填：
  ```
  https://cdn.jsdmirror.com/gh/cy0416/tv@master/tvbox/author/xxx.json
  ```
  例：`https://cdn.jsdmirror.com/gh/cy0416/tv@master/tvbox/author/new.json`
- **备用直链**（GitHub raw）：`https://raw.githubusercontent.com/cy0416/tv/master/tvbox/author/xxx.json`

## 四、说明
- 接口**配置文件已到你手里**（作者删库你仍有副本），但内部引用的**第三方站点/jar 仍在作者或第三方服务器**，站点失效接口仍会挂。
- 是否真正能播，需在电视端实测；失效时优先用活跃的 tvbox2 接口（bj/zz/yyds 等）。
