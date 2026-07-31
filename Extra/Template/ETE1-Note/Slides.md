---
title: <% tp.config.target_file.basename %>
cssclasses: [slides]
tags: [slides, 道場/傳題/1新民班]
date_created: <% tp.file.creation_date('yyyy-MM-DDTHH:mm:ssZZ') %>
date_modified: 2026-06-17T10:01:28+08:00
status: 🌱 下種
share_link: ""
share_updated: ""
share_unencrypted: true
theme: black
transition: concave
css: [../../.obsidian/snippets/mater.css, ../../.obsidian/snippets/slides_extended_css.css]
ObsidianUIMode: source
obsidianEditingMode: source
---
^frontmatter


# <% tp.config.target_file.basename %>
<!-- .element: class="r-fit-text" -->

<div class="hide-from-slides">

> [!table|share-none]
>> [!table|right]
>> 狀態：`INPUT[inlineSelect(option(🌱 下種), option(🪴 發展), option(👣 追蹤), option(🥾 交付), option(✅ 完成), option(🌲 常青), option(❌ 取消), showcase):status]`
>> 主題：`INPUT[inlineSelect(option(beige), option(black), option(blood), option(dracula), option(league), option(moon), option(night), option(serif), option(simple), option(sky), option(solarized), option(white), showcase):theme]`
>> 切換：`INPUT[inlineSelect(option(none), option(fade), option(slide), option(convex), option(concave), option(zoom), showcase):transition]`
>
> [up:: [[傳題]], [[Base_投影片.base|投影片]]]  [[#^current-note-path|目前位置]] [[Slides|模板]]
> ```meta-bind-button
> style: default
> label: 開啟簡報
> tooltip: 'presentation'
> id: presentation
> hidden: false
> action:
>   type: command
>   command: slides:start
> ```
>
> ```meta-bind-button
> style: default
> label: RevealJS
> tooltip: 'Slides Extended: Show slide preview'
> id: RevealJS
> hidden: false
> action:
>   type: command
>   command: slides-extended:open-preview
> ```

> [!right]
> 修改@`$=dv.span(dv.current().date_modified)`

</div><!-- element style="display: none;" -->

---

## 大綱
<!-- .element: style="display:none;" -->

> [!flex]
>> [!outline|slide-hide] Slides
>> - [[#大綱]]
>> - [[#Title_and_Contents]]
>> - [[#Contents with Image]]
>> - [[#Two Columns of List]]
><!-- .element: style="display:none;" -->
>
>> [!outline|color-purple|reading-view-hide] Slides
>> - [大綱](#/1)
>> - [Title_and_Contents](#/2)
>> - [Contents with Image](#/3)
>> - [Two Columns of List](#/5)


```dataviewjs
// 取得當前檔案內容
const file = dv.current().file;
const content = await app.vault.read(app.vault.getAbstractFileByPath(file.path));
const lines = content.split("\n");

// 初始化參數
let currentSlide = -2; // 簡報頁碼通常從 0 或 1 開始
const headings_reveal = [];

// 遍歷所有行，計算頁碼與標題
for (let i = 0; i < lines.length; i++) {
	const line = lines[i].trim();

	// 檢測水平分頁 (---) 或垂直分頁 (----)
	// 這裡視你的簡報結構而定，若無垂直分頁，只需判斷 "---"
	if (line === "---" || line === "----") {
		currentSlide++;
		continue;
	}

	// 匹配標題 (##, ###, ####)
	const m = line.match(/^(#{2,4})\s+(.*)/);
	if (!m) continue;

	const level = m[1].length;
	const title = m[2].trim();

	headings_reveal.push({ level, title, slide: currentSlide });
}

// 產生輸出區塊
let slide_block = "";
let reading_block = "";
const baseLevel = 2; // 基準層級

for (const h of headings_reveal) {
	const indent = "  ".repeat(h.level - baseLevel);

	// Slide 連結 (Reveal.js 格式)
	const slideLink = h.level === 2 ? `(#/${h.slide})` : `(#/${h.slide}/0/0)`;
	slide_block += `>> ${indent}- [${h.title}]${slideLink}\n`;

	// Reading View 連結 (Obsidian 內部連結)
	reading_block += `>> ${indent}- [[#${h.title}]]\n`;
}

// 組合最終 Markdown 結構
const output = `
> [!flex]
>> [!outline|slide-hide] **大綱 (預覽)**
${reading_block}
>
>> [!outline|color-blue|reading-view-hide] **大綱 (簡報)**
${slide_block}
`;

// 輸出到頁面
dv.span(output);
```
<!-- .element: style="display:none;" -->

```dataviewjs [1-2]
const file = dv.current().file;
const content = await this.app.vault.read(app.vault.getAbstractFileByPath(file.path));
const lines = content.split("\n");
const file_name = file?.name;

// 解析 slide 頁碼（從 HTML 註解抓 (#/n)）
function getSlideIndex(i) {
	for (let j = i - 1; j >= 0; j--) {
		// 修改：增加對強制分隔符 --- 的捕捉，或者確保能抓到更完整的錨點
		const m = lines[j].match(/#\/(\d+)/);
		if (m) return m[1];
	}
	// 如果找不到，給一個預設值，而不是 return null
	return "1";
}

// 擷取標題
const headings_reveal = [];
for (let i = 0; i < lines.length; i++) {
	const line = lines[i];
	const m = line.match(/^(#{2,4})\s+(.*)/);
	if (!m) continue;

	const level = m[1].length; // ## = 2, ### = 3, #### = 4
	const title = m[2].trim();
	const slide = getSlideIndex(i);

	if (slide) {
		headings_reveal.push({ level, title, slide });
	}
}

// 基準層級
const baseLevel = 2;

// 產生階層清單
let slide_block = "";
let reading_block = "";

for (const h of headings_reveal) {
	const indent = "  ".repeat(h.level - baseLevel);
	const link =
		h.level === 2
			? `(#/${h.slide})`
			: `(#/${h.slide}/0/0)`;
	// Slide Mode 標記，裡面是 Reveal/Slide 專用標記
	slide_block += `>> ${indent}- [${h.title}]${link}\n`;
	// Reading View 標記
	reading_block += `>> ${indent}- [[#${h.title}]]\n`;
}

// 輸出，無法顯示在reading-view，因為slide_block無法渲染
let link_block = "> [!flex] \n>> [!outline|slide-hide] " + file_name + "\n" + reading_block + ">\n>> [!outline|color-purple|reading-view-hide] " + file_name + "\n>>\n>>" + slide_block
// 輸出，可以顯示在reading-view，但slide_block要手動貼上
let code_block = "> [!flex] \n>> [!outline|slide-hide] " + file_name + "\n" + reading_block + ">\n>> [!outline|color-blue|reading-view-hide] " + file_name + "\n>>\n>> ````text\n" + slide_block + ">> ````\n";
let code_pure = `
\`\`\`\`markdown
> [!flex]
>> [!outline|slide-hide] ${file_name}
${reading_block}><!-- .element: style=\"display:none;\" -->
>
>> [!outline|color-purple|reading-view-hide] ${file_name}
${slide_block}
\`\`\`\`
`;

// 顯示為大綱及文字區塊
dv.paragraph(code_pure);
```
<!-- .element: style="display:none;" -->

---
<!-- .element: style="background:#333;" -->
<!-- 第 (#/2) 頁 -->

## Title_and_Contents

- Contents
- 歸根<u>認<span class="mater"></span></u>之領航員
- 歸根<mark>認<span class="mater"></span></mark>之領航員
- 歸*根*<i>認<span class="mater"></span></i>之領航員
- 歸**根**<strong>認<span class="mater"></span></strong>之領航員

%%
Note:
This will only display in the notes window.
%%

[回大綱](#/1)

[[#大綱|回大綱]]

---

<!-- 第 (#/3) 頁 -->

## Contents with Image

> [!flex]
>> [!rocket]
>> - list 1
>
> ![Image|300](https://picsum.photos/id/1006/500/300)

[回大綱](#/1)

[[#大綱|回大綱]]

---

Gatsby believed in the green light, the orgastic future that year by year recedes before us.
It eluded us then, but that’s no matter — tomorrow we will run faster, stretch our arms further...
And one fine morning — So we beat on.<!-- .element: style="font-size: 46px" align="justify" -->


[回大綱](#/1)

[[#大綱|回大綱]]

---

<!-- 第 (#/5) 頁 -->

## Two Columns of List

<split>

- left items 1
- right **items** 1
- [last](#/grand-finale)
+ [Open link](https://hakim.se)<!-- .element data-preview-link -->

* any item

</split>

[回大綱](#/1)

[[#大綱|回大綱]]

---

<!-- .slide id="grand-finale" -->

敬祝
<!-- .element: style="font-size: 2em" -->

聖凡如意
<!-- .element: class="r-fit-text" -->

---

<!-- .slide: data-visibility="hidden" -->

# 課後總結


[[#大綱|回大綱]]