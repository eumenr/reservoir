---
title: Home
date_created: 2026-07-31T15:12:59+08:00
date_modified: 2026-07-31T15:15:19+08:00
status: 🌱 下種
theme: black
transition: concave
css: [../../.obsidian/snippets/mater.css]
ObsidianUIMode: source
obsidianEditingMode: source
---
^frontmatter

# Home

- [[大學選讀(三)]]

## 投影片

```dataview
Table
	split(status, " ")[0] as 狀態,
	split(file.tags[1], "/")[2] as 標籤,
	dateformat(file.mtime, "MM-dd") as 日期
from #slides and !"Extra/Template" and !"Space/S4-Archive" and !".obsidian" and !"Space/S1-Project/NTBT/.obsidian" and !"Space/S1-Project/NTBT/Extra/Template"
Where contains(file.tags, "道場") and !contains(file.tags, "明德")
Sort file.mtime DESC
limit 8
```