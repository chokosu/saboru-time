# saboru-timer

摸鱼计时器。单文件 HTML app，莫兰迪绿配色，无框架，纯原生 JS。

## 文件结构

- `摸鱼计时器.html` — 唯一的源文件，所有代码在这里改
- `index.html` — 与上面完全同步，供 GitHub Pages 托管用
- `CHANGELOG.md` — 每次改动后更新

**每次改完 `摸鱼计时器.html`，必须同步到 `index.html`：**
```bash
cp 摸鱼计时器.html index.html
```

## 技术栈

- 纯 HTML + CSS + 原生 JS，无任何依赖
- 数据全部存在 `localStorage`（key: `muyuHistory`, `muyuChecklist`）
- 配色：背景 `#edeee8`，主绿 `#7a9b7c`

## 核心数据结构

```js
// localStorage['muyuHistory']
{
  "2026-06-18": {
    clockIn: <timestamp>,
    clockOut: <timestamp|undefined>,
    tasks: [{ id, name, start, end, type:'work'|'slack'|'lunch', status:'active'|'paused'|'done' }],
    notes: [{ id, text, ts }],
    fishCount: <n>,
    vacation: <bool>,
    checks: { [checklistItemId]: bool }
  }
}

// localStorage['muyuChecklist']  — 固定任务模板
[{ id, name }]
```

## 核心函数

- `calcDay(tasks, clockIn, refNow)` — 计算当日工作/摸鱼/午休时长
- `mergeWorkMs(tasks, refNow)` — 合并并行 work 任务区间，去重
- `tick()` — 每秒执行，驱动所有实时更新（`setInterval(tick, 1000)`）
- `computeStats()` — 更新统计卡片
- `renderTimeline()` — 渲染时间轴
- `saveToday()` / `loadToday()` — localStorage 读写

## 布局

- 移动端：单列
- 桌面端（≥780px）：两栏 Grid
  - `.col-top`：倒计时 + 进度条，横跨全宽
  - `.col-left`：操作区（午休横幅、进行中任务、清单）
  - `.col-right`：数据区（统计、摸鱼指数、时间轴、历史记录）

## 协作流程

1. 改 `摸鱼计时器.html`
2. `cp 摸鱼计时器.html index.html`
3. 更新 `CHANGELOG.md` 顶部加一条记录
4. `git commit` + `git push`
5. 功能积累够了：`gh release create vX.X.X`

## 部署

GitHub Pages：https://chokosu.github.io/saboru-time/
仓库地址：https://github.com/chokosu/saboru-time
