---
title: "Markdown"
date: 2019-12-13T02:40:57+08:00
tags:
- Tool
---

据 GitHub Flavored Markdown（GFM）官方文档介绍，Markdown 是由约翰·格鲁伯（John Gruber）在亚伦·斯沃茨（Aaron Swartz）的帮助下开发，并在 2004 年发布的标记语言。

其设计灵感主要来源于纯文本电子邮件的格式，目标是让人们能够使用易读、易写的纯文本格式编写文档，而且这些文档可以转换为 HTML（Hyper Text Markup Language，超文本标记语言）文档。

<!--more-->

起初 Markdown 主要用于网络写作，后来人们希望 Markdown 能够应用到更多的领域，如写书、记笔记、写文档、写幻灯片等。

由于 Markdown 本身功能有限，一些特定的需求和场景无法被满足，因此产生了许多扩展语法，这些语法在基础语法之上新增了如表格、任务列表、围栏代码块等功能。


## 历史

- 2004 年，Markdown 发布，作者是 John Gruber。
- 2006 年，Pandoc＇s Markdown 发布，作者是 John MacFarlane。 此版本对 Markdown 语法有额外的扩充和些许修正，这使 Markdown 可以转换为更多的文件格式，Pandoc 堪称文件转换领域的「瑞士军刀」。
- 2011 年，MultiMarkdown（简称 MMD）发布，作者是 Fletcher T.Penney。 此版本让 Markdown 可以转换为更多的文件格式，包括 HTML/XHTML、LATEX、OpenDocument、OPML（Outline Processor Markup Language，大纲处理标记语言）。
- 2013 年，Markdown Extra 发布，作者是 Michel Fortin。 此版本最初使用 PHP 语言实现，新增了围栏代码块、具有 id/class 属性的元素、表格、任务列表、脚注、缩写等功能。
- 2014 年，CommonMark 规范发布，主要作者是 Jeff Atwood 和 John MacFarlane。 CommonMark 旨在为人们提供一个标准的 Markdown 语法规范和参考实现。 Markdown 标准化工作开始于 2012 年，2014 年 9 月，由于 John Gruber 反对在这一工作中继续使用「Markdown」这个名字，其被更名为 CommonMark。
- 2017 年，GitHub 发布了 GitHub Flavored Markdown，即 GFM。 此版本遵循 CommonMark 规范，新增了围栏代码块、表格、删除线、自动链接、Emoji 表情和任务列表等功能，是目前使用最广泛的版本。


总之，版本之多  John Gruber 认为合理，未做约束，作出约束的是[ CommonMark](http://commonmark.org/) ——它为 Markdown 提出了一个标准的、明确的语法规范，以及一套全面的测试，根据此规范可以验证 Markdown 的实现结果，GitHub Flavored Markdown（GFM）遵循的就是 CommonMark 规范。

**编辑器：** Typora、熊掌记、Ulysse


## 心得

+ **表情符号参考**: http://www.webpagefx.com/tools/emoji-cheat-sheet/
+  **锚点（书签）-> 就是URL。**
   + **JS 插件**: 
   + `<sup><a class="xxx" href="xxx"></a></sup>`  +  `<a class="xxx">[1].xxxxx<>` 
   + 下标对应使用`<sub></sub>`
+ **点击展开**: `<details><summary>点击展开</summary><a>xxxxx</a></details>`
+ **一些来自他人的建议：**
  1. **幻灯片插件**——reveal.js
  2. **邮箱渲染插件**——Markdown Here
  3. **交互性文档**——Jupyter Notebook & R Markdown
      1. R Markdown 的内容请参考官方文档 [https://rmarkdown.rstudio.com/](https://rmarkdown.rstudio.com/)
      2. R Markdown 权威指南 [https://bookdown.org/yihui/rmarkdown/](https://bookdown.org/yihui/rmarkdown/)
  4. **网页便携程序**——md-page
      1. url：[https://github.com/oscarmorrison/md-page](https://github.com/oscarmorrison/md-page。)
  5. **项目文档书写：**
      1. **Mkdocs**: 如 Google 的 python-fire.
      2. **VuePress**: 集成了 Google Analytics，以及基于 Git 的「最后更新时间」功能，见[https://vuepress.vuejs.org/zh/guide/](https://vuepress.vuejs.org/zh/guide/。)
  6. **Hexo**
  7. **Gitbook**——支持 Markdown 和 AsciiDoc 语法，可以导出静态站点或电子书（支持 PDF、ePub、mobi 等格式）
  8. 付费渠道
- 空格尽量用 `_` 来替换, 因为在网页的编码中, ' '代表`%20`, 出来的网址会冗杂.

## mermaid 
- [mermaid](https://mermaid-js.github.io/mermaid/#/)  

## Code list[<sup>1</sup>](#jump)

| code           | type                                |
| :--------------: | :-----------------------------------: |
| 1c             | 1C:Enterprise (v7, v8)              |
| abnf           | Augmented Backus-Naur Form          |
| accesslog      | Access log                          |
| actionscript   | ActionScript                        |
| ada            | Ada                                 |
| angelscript    | AngelScript                         |
| apache         | Apache                              |
| applescript    | AppleScript                         |
| arcade         | ArcGIS Arcade                       |
| arduino        | Arduino                             |
| armasm         | ARM Assembly                        |
| asciidoc       | AsciiDoc                            |
| aspectj        | AspectJ                             |
| autohotkey     | AutoHotkey                          |
| autoit         | AutoIt                              |
| avrasm         | AVR Assembler                       |
| awk            | Awk                                 |
| axapta         | Microsoft Axapta (now Dynamics 365) |
| bash           | Bash                                |
| basic          | Basic                               |
| bnf            | Backus–Naur Form                    |
| brainfuck      | Brainfuck                           |
| cal            | C/AL                                |
| capnproto      | Cap’n Proto                         |
| ceylon         | Ceylon                              |
| clean          | Clean                               |
| clojure-repl   | Clojure REPL                        |
| clojure        | Clojure                             |
| cmake          | CMake                               |
| coffeescript   | CoffeeScript                        |
| coq            | Coq                                 |
| cos            | Cache Object Script                 |
| cpp            | C++                                 |
| crmsh          | crmsh                               |
| crystal        | Crystal                             |
| cs             | C#                                  |
| csp            | CSP                                 |
| css            | CSS                                 |
| d              | D                                   |
| dart           | Dart                                |
| delphi         | Delphi                              |
| diff           | Diff                                |
| django         | Django                              |
| dns            | DNS Zone file                       |
| dockerfile     | Dockerfile                          |
| dos            | DOS .bat                            |
| dsconfig       | dsconfig                            |
| dts            | Device Tree                         |
| dust           | Dust                                |
| ebnf           | Extended Backus-Naur Form           |
| elixir         | Elixir                              |
| elm            | Elm                                 |
| erb            | ERB (Embedded Ruby)                 |
| erlang-repl    | Erlang REPL                         |
| erlang         | Erlang                              |
| excel          | Excel                               |
| fix            | FIX                                 |
| flix           | Flix                                |
| fortran        | Fortran                             |
| fsharp         | F#                                  |
| gams           | GAMS                                |
| gauss          | GAUSS                               |
| gcode          | G-code (ISO 6983)                   |
| gherkin        | Gherkin                             |
| glsl           | GLSL                                |
| gml            | GML                                 |
| go             | Golang                              |
| golo           | Golo                                |
| gradle         | Gradle                              |
| groovy         | Groovy                              |
| haml           | Haml                                |
| handlebars     | Handlebars                          |
| haskell        | Haskell                             |
| haxe           | Haxe                                |
| hsp            | HSP                                 |
| htmlbars       | HTMLBars                            |
| http           | HTTP (Header Plaintext)             |
| hy             | Hy                                  |
| inform7        | Inform 7                            |
| ini            | TOML, also INI                      |
| irpf90         | IRPF90                              |
| isbl           | ISBL                                |
| java           | Java                                |
| javascript     | JavaScript                          |
| jboss-cli      | jboss-cli                           |
| json           | JSON / JSON with Comments           |
| julia-repl     | Julia REPL                          |
| julia          | Julia                               |
| kotlin         | Kotlin                              |
| lasso          | Lasso                               |
| ldif           | LDIF                                |
| leaf           | Leaf                                |
| less           | Less                                |
| lisp           | Lisp                                |
| livecodeserver | LiveCode                            |
| livescript     | LiveScript                          |
| llvm           | LLVM IR                             |
| lsl            | LSL (Linden Scripting Language)     |
| lua            | Lua                                 |
| makefile       | Makefile                            |
| markdown       | Markdown                            |
| mathematica    | Mathematica                         |
| matlab         | Matlab                              |
| maxima         | Maxima                              |
| mel            | MEL                                 |
| mercury        | Mercury                             |
| mipsasm        | MIPS Assembly                       |
| mizar          | Mizar                               |
| mojolicious    | Mojolicious                         |
| monkey         | Monkey                              |
| moonscript     | MoonScript                          |
| n1ql           | N1QL                                |
| nginx          | Nginx                               |
| nimrod         | Nim (formerly Nimrod)               |
| nix            | Nix                                 |
| nsis           | NSIS                                |
| objectivec     | Objective-C                         |
| ocaml          | OCaml                               |
| openscad       | OpenSCAD                            |
| oxygene        | Oxygene                             |
| parser3        | Parser3                             |
| perl           | Perl                                |
| pf             | pf.conf                             |
| pgsql          | PostgreSQL SQL dialect and PL/pgSQL |
| php            | PHP                                 |
| plaintext      | Plaintext                           |
| pony           | Pony                                |
| powershell     | PowerShell                          |
| processing     | Processing                          |
| profile        | Python profile                      |
| prolog         | Prolog                              |
| properties     | Properties                          |
| protobuf       | Protocol Buffers                    |
| puppet         | Puppet                              |
| purebasic      | PureBASIC                           |
| python         | Pythin                              |
| q              | Q                                   |
| qml            | QML                                 |
| r              | R                                   |
| reasonml       | ReasonML                            |
| rib            | RenderMan RIB                       |
| roboconf       | Roboconf                            |
| routeros       | Microtik RouterOS script            |
| rsl            | RenderMan RSL                       |
| ruby           | Ruby                                |
| ruleslanguage  | Oracle Rules Language               |
| rust           | Rust                                |
| sas            | SAS                                 |
| scala          | Scala                               |
| scheme         | Scheme                              |
| scilab         | Scilab                              |
| scss           | SCSS                                |
| shell          | Shell Session                       |
| smali          | Smali                               |
| smalltalk      | Smalltalk                           |
| sml            | SML (Standard ML)                   |
| sqf            | SQF                                 |
| sql            | SQL (Structured Query Language)     |
| stan           | Stan                                |
| stata          | Stata                               |
| step21         | STEP Part 21                        |
| stylus         | Stylus                              |
| subunit        | SubUnit                             |
| swift          | Swift                               |
| taggerscript   | Tagger Script                       |
| tap            | Test Anything Protocol              |
| tcl            | Tcl                                 |
| tex            | TeX                                 |
| thrift         | Thrift                              |
| tp             | TP                                  |
| twig           | Twig                                |
| typescript     | TypeScript                          |
| vala           | Vala                                |
| vbnet          | VB.NET                              |
| vbscript-html  | VBScript in HTML                    |
| vbscript       | VBScript in HTML                    |
| verilog        | Verilog                             |
| vhdl           | VHDL                                |
| vim            | Vim Script                          |
| x86asm         | Intel x86 Assembly                  |
| xl             | XL                                  |
| xml            | HTML, XML                           |
| xquery         | XQuery                              |
| yaml           | YAML                                |
| zephir         | Zephir                              |

</br>

#![](https://z3.ax1x.com/2021/06/28/RNt0kn.png)

<div id="jump">[1]. Code list : https://terryl.in/en/highlight-js-html-code-language-list-for-syntax-highlighting/#how-it-works</div>

