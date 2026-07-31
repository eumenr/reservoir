---

<grid drag="90 15" drop="5 5" flow="row" animate="semi-fade-out" frag="1"  bg="transparent">

<% tp.system.prompt('Two-Columns with a Head\n頁面標題','','頁面標題') %>
<!-- .element: class="r-fit-text" -->
<!-- .element: style="font-size: 1.2em" -->

</grid><grid drag="43 60" drop="5 25" flow="col" align="top" bg="transparent">
<!-- 左半 -->

- left_items 1
+ left_items 2
+ left_items 3

</grid><grid drag="43 60" drop="-5 25" flow="col" frag="8" align="top" animate="slideUpIn" bg="transparent">
<!-- 右半 -->

- right_items

</grid><grid drag="90 5" drop="5 -5" flow="row" bg="transparent">
<!-- 下 -->
[回大綱](#/1)
<!-- element style="text-align: right; " -->
[[#^toc-<% tp.file.creation_date("YYYY-MMDD-kkmmss") %>|回大綱]]<!-- element style="display: none;" -->

</grid>
