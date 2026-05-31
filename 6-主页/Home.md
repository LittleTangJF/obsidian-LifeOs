---
created: 2026-05-31
tags: [home, index]
---

# Home

## 快速收集
- **快记**（`Ctrl+Shift+N`）— 在收件箱新建一篇笔记
- **随手记**（`Ctrl+Shift+M`）— 弹窗输入一行，追加到收件箱

## 最近修改
```dataview
TABLE file.mtime AS "最后修改"
FROM "" SORT file.mtime DESC LIMIT 10
```

## 知识库总览
```dataview
TABLE rows.file.len AS "笔记数"
FROM ""
GROUP BY file.folder AS "文件夹"
SORT rows.file.len DESC
```

## 内容地图
- [[MOC-前端开发]] — React / JavaScript / TypeScript / CSS / Electron / 算法
- [[MOC-投资]] — 股票知识 / 龙头战法 / 短线操盘
- [[MOC-工作]] — WOT 分享 / 演讲排练 / 团队

## 项目中的笔记
```dataview
TABLE rows.file.len AS "笔记数", rows.file.mtime AS "最近更新"
FROM "1. 项目"
GROUP BY file.folder AS "项目子模块"
SORT rows.file.len DESC
```
