---
title: learning python outloud
author: Jen Richmond
date: '2024-07-04'
slug: []
categories: []
tags: []
---

<script src="{{< blogdown/postref >}}index_files/core-js/shim.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/react/react.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/react/react-dom.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/reactwidget/react-tools.js"></script>
<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<link href="{{< blogdown/postref >}}index_files/reactable/reactable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/reactable-binding/reactable.js"></script>

When you are exploring a far off land and only know a tiny bit of the language they speak there, you ALWAYS carry a little dictionary with commonly used phrases translated from the language you speak into the other language. It is important to know how to ask someone where the toilets are while you are travelling!

I have just started learning Python with Posit Academy in the lead up to \#positconf2024 and I am trying to approach in the same way I would if I was learning French. A dictionary that helps me link functions I know in R to new functions I am learning in Python could be handy.

Linking new concepts to old concepts is also a useful learning strategy. Research in Psychology tells us that memory is relational; the brain represents memories as networks of representations. If you can link something that you are trying to learn to something you already know, you are much more likely to remember that new thing into the long term.

<div class="figure">

<img src="https://chinablueart.com/wp-content/uploads/MemoryNetwork-ICompilation.jpg" alt="Art credit: China Blue Art, Memory Network I" width="50%" />
<p class="caption">
<span id="fig:unnamed-chunk-1"></span>Figure 1: Art credit: China Blue Art, Memory Network I
</p>

</div>

Of course, there are probably a million R to Python dictionaries on the internet; why am I creating a new one for me?

That’s because research in Psychology has shown that generative learning and retrieval practice are more effective strategies for remembering things into the long term, than are learning strategies that involve passive review of materials that were created by someone else.

To create a dictionary of functions as I learn new things in Python, I need to retrieve the equivalent function in R from memory and actively evaluate the similarities and differences between the Python and R versions. That process of retrieving and using the concepts I already know, strengthens both my knowledge of R, and links my new Python understanding to it.

So I am starting a [googlesheet](https://docs.google.com/spreadsheets/d/1Vf1AUqdAS_rsyaZw18OxyaIPOoiv9WvOzvILdSRtU6U/edit?usp=sharing) to keep track of new things I am learning how to do in Python and their R equivalents. Maybe I can read that googlesheet in here using the `googlesheets4` package and make it display in a searchable table using `gt`.

``` r
cheat_sheet %>%
  gt() %>%
  tab_header("My R vs Python cheatsheet") %>%
  opt_interactive(use_search = TRUE)
```

<div id="stvkaejbwf" class=".gt_table" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#stvkaejbwf table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
&#10;#stvkaejbwf thead, #stvkaejbwf tbody, #stvkaejbwf tfoot, #stvkaejbwf tr, #stvkaejbwf td, #stvkaejbwf th {
  border-style: none;
}
&#10;#stvkaejbwf p {
  margin: 0;
  padding: 0;
}
&#10;#stvkaejbwf .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}
&#10;#stvkaejbwf .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}
&#10;#stvkaejbwf .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}
&#10;#stvkaejbwf .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}
&#10;#stvkaejbwf .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}
&#10;#stvkaejbwf .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#stvkaejbwf .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}
&#10;#stvkaejbwf .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}
&#10;#stvkaejbwf .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}
&#10;#stvkaejbwf .gt_column_spanner_outer:first-child {
  padding-left: 0;
}
&#10;#stvkaejbwf .gt_column_spanner_outer:last-child {
  padding-right: 0;
}
&#10;#stvkaejbwf .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}
&#10;#stvkaejbwf .gt_spanner_row {
  border-bottom-style: hidden;
}
&#10;#stvkaejbwf .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}
&#10;#stvkaejbwf .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}
&#10;#stvkaejbwf .gt_from_md > :first-child {
  margin-top: 0;
}
&#10;#stvkaejbwf .gt_from_md > :last-child {
  margin-bottom: 0;
}
&#10;#stvkaejbwf .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}
&#10;#stvkaejbwf .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#stvkaejbwf .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}
&#10;#stvkaejbwf .gt_row_group_first td {
  border-top-width: 2px;
}
&#10;#stvkaejbwf .gt_row_group_first th {
  border-top-width: 2px;
}
&#10;#stvkaejbwf .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#stvkaejbwf .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}
&#10;#stvkaejbwf .gt_first_summary_row.thick {
  border-top-width: 2px;
}
&#10;#stvkaejbwf .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#stvkaejbwf .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#stvkaejbwf .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}
&#10;#stvkaejbwf .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}
&#10;#stvkaejbwf .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}
&#10;#stvkaejbwf .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#stvkaejbwf .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}
&#10;#stvkaejbwf .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#stvkaejbwf .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}
&#10;#stvkaejbwf .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#stvkaejbwf .gt_left {
  text-align: left;
}
&#10;#stvkaejbwf .gt_center {
  text-align: center;
}
&#10;#stvkaejbwf .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}
&#10;#stvkaejbwf .gt_font_normal {
  font-weight: normal;
}
&#10;#stvkaejbwf .gt_font_bold {
  font-weight: bold;
}
&#10;#stvkaejbwf .gt_font_italic {
  font-style: italic;
}
&#10;#stvkaejbwf .gt_super {
  font-size: 65%;
}
&#10;#stvkaejbwf .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}
&#10;#stvkaejbwf .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}
&#10;#stvkaejbwf .gt_indent_1 {
  text-indent: 5px;
}
&#10;#stvkaejbwf .gt_indent_2 {
  text-indent: 10px;
}
&#10;#stvkaejbwf .gt_indent_3 {
  text-indent: 15px;
}
&#10;#stvkaejbwf .gt_indent_4 {
  text-indent: 20px;
}
&#10;#stvkaejbwf .gt_indent_5 {
  text-indent: 25px;
}
</style>
<div style="font-family:system-ui, &#39;Segoe UI&#39;, Roboto, Helvetica, Arial, sans-serif;border-top-style:solid;border-top-width:2px;border-top-color:#D3D3D3;padding-bottom:8px;">
<div class="gt_heading gt_title gt_font_normal" style="text-size:bigger;">My R vs Python cheatsheet</div>
<div class="gt_heading gt_subtitle gt_bottom_border"></div>
</div>
<div id="stvkaejbwf" class="reactable html-widget" style="width:auto;height:auto;"></div>
<script type="application/json" data-for="stvkaejbwf">{"x":{"tag":{"name":"Reactable","attribs":{"data":{"how to":["assign variables","install packages","load packages","use functions from a package","use functions that are built in","get help","random number between 0-1","random number between range","get frequencies"],"R":["my_name <- Jenny","install.packages(\"tidyverse\")","library(tidyverse)","clean_names(data) OR janitor::clean_names(data)","round(x, digits = 3)","?name_of_package_function","runif(1)","runif(1, min = 1, max = 100)","janitor::tabyl(data) %>% arrange(n)"],"python":["my_name = Jenny","pip install numpy","import numpy","numpy.sqrt(data) OR np.sqrt(data)","round(x, ndigits = 3)","help(name_of_package_function)","random.random()","random.ranint(1,100)","pd.value_counts(data, ascending = True)"],"differences":[null,null,"use import numpy as np if you want to use an alias to type less","once you load a package in R with library(packagename), R just knows to use functions from that package. In python, you need to tell it what package each function comes from-  requires namespace","no need to use namespace in python for functions that are built in (https://docs.python.org/3/library/functions.html)",null,"requires that you have loaded the random package with import random",null,"pd refers to pandas (will only work if you have said import pandas as pd, True works but TRUE does not"]},"columns":[{"id":"how to","name":"how to","type":"character","style":"function(rowInfo, colInfo) {\nconst rowIndex = rowInfo.index + 1\n}","cell":["assign variables","install packages","load packages","use functions from a package","use functions that are built in","get help","random number between 0-1","random number between range","get frequencies"],"html":true,"align":"left","headerStyle":{"font-weight":"normal"}},{"id":"R","name":"R","type":"character","style":"function(rowInfo, colInfo) {\nconst rowIndex = rowInfo.index + 1\n}","cell":["my_name &lt;- Jenny","install.packages(\"tidyverse\")","library(tidyverse)","clean_names(data) OR janitor::clean_names(data)","round(x, digits = 3)","?name_of_package_function","runif(1)","runif(1, min = 1, max = 100)","janitor::tabyl(data) %&gt;% arrange(n)"],"html":true,"align":"left","headerStyle":{"font-weight":"normal"}},{"id":"python","name":"python","type":"character","style":"function(rowInfo, colInfo) {\nconst rowIndex = rowInfo.index + 1\n}","cell":["my_name = Jenny","pip install numpy","import numpy","numpy.sqrt(data) OR np.sqrt(data)","round(x, ndigits = 3)","help(name_of_package_function)","random.random()","random.ranint(1,100)","pd.value_counts(data, ascending = True)"],"html":true,"align":"left","headerStyle":{"font-weight":"normal"}},{"id":"differences","name":"differences","type":"character","style":"function(rowInfo, colInfo) {\nconst rowIndex = rowInfo.index + 1\n}","cell":["NA","NA","use import numpy as np if you want to use an alias to type less","once you load a package in R with library(packagename), R just knows to use functions from that package. In python, you need to tell it what package each function comes from-  requires namespace","no need to use namespace in python for functions that are built in (https://docs.python.org/3/library/functions.html)","NA","requires that you have loaded the random package with import random","NA","pd refers to pandas (will only work if you have said import pandas as pd, True works but TRUE does not"],"html":true,"align":"left","headerStyle":{"font-weight":"normal"}}],"searchable":true,"defaultPageSize":10,"showPageSizeOptions":false,"pageSizeOptions":[10,25,50,100],"paginationType":"numbers","showPagination":true,"showPageInfo":true,"minRows":1,"showSortable":true,"height":"auto","theme":{"color":"#333333","backgroundColor":"#FFFFFF","stripedColor":"rgba(128,128,128,0.05)","style":{"fontFamily":"system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif"},"headerStyle":{"borderTopStyle":"solid","borderTopWidth":"2px","borderTopColor":"#D3D3D3","borderBottomStyle":"solid","borderBottomWidth":"2px","borderBottomColor":"#D3D3D3"}},"elementId":"stvkaejbwf","dataKey":"9982aefa6b9238133ef8b0b2381bfb12"},"children":[]},"class":"reactR_markup"},"evals":["tag.attribs.columns.0.style","tag.attribs.columns.1.style","tag.attribs.columns.2.style","tag.attribs.columns.3.style"],"jsHooks":[]}</script>
</div>
