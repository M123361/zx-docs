---
title: Python python-docx使用
icon: fas fa-list
author: 周子力
order: 50
category:
  - 教学文档
tag:
  - Python
---

# Python Python-docx使用

## 1.Python-docx简介

Microsoft Word是最常用的文档处理工具之一，但有时需要以编程方式生成或修改Word文档。Python中有一个`python-docx`的库，它允许创建、修改和操作Word文档。

本文将详细介绍`python-docx`库的用法，包括如何创建文档、添加文本、格式化文本、插入表格和图片等。

[API文档参考](https://python-docx.readthedocs.io/en/latest/index.html)

## 2.python-docx安装

```
pip install python-docx
pip install  -i https://pypi.tuna.tsinghua.edu.cn/simple python-docx 
```

## 3.python-docx使用-写

### 3.1 创建一个Word文档

```py
from docx import Document  #导入包
doc = Document() #创建一个Document对象
```

### 3.2 添加标题和段落

```python
#添加标题
doc.add_heading('网安文档',0)

#添加段落
doc.add_paragraph('我是曲师大网络空间安全学学院学生')

#添加段落中的一个节段
p=doc.add_paragraph()
p.add_run('这是一个节段')
```

![picture 1](https://oss.docs.z-xin.net/216719e14d402505a62bacbd7338e52d5ef715c6025fd2c55c1c52fdadc7f3f2.png)  

(1)文档对象（Document）：Document对象是Python-docx库的核心，它代表了一个完整的Word文档。通过Document对象，可以访问和修改文档中的其他部分，如段落、表格、图片等。
(2)段落（Paragraph）：段落是文档中最基本的文本组织单位。每个Paragraph对象代表文档中的一个段落。通过它可以获取和设置段落的文本内容、样式（如字体、字号、颜色等）以及格式（如对齐方式、缩进、间距等）。
(3)文本（Run）：在Python-docx中，Run对象表示具有相同格式的连续文本序列。一个段落可以包含多个运行，每个运行可以有不同的样式和格式。通过Run对象，你可以获取和设置文本的格式和样式，如字体、字号、颜色、粗体、斜体等。
(4)表格（Table）：表格是文档中用于组织数据的结构。每个Table对象代表一个表格，包含行（Row）和单元格（Cell）。通过表格对象，你可以获取和设置表格的样式、行高、列宽以及单元格的内容。
(5)图片（InlineShape）：InlineShape对象用于表示文档中的内联形状。通过遍历文档中的InlineShape对象，你可以找到并处理文档中的图片，如获取图片的路径、大小等信息。
(6)节（Section）：节是文档中的一个逻辑部分，通常用于控制页面设置（如页边距、纸张大小等）和页眉页脚的显示。通过Document.sections属性，你可以获取文档中的所有节，并设置每节的属性。
(7)页眉和页脚（Header 和 Footer）：页眉和页脚分别用于在文档的每一页的顶部和底部显示相同的内容。通过Section对象的header和footer属性，你可以获取和设置文档的页眉和页脚内容。

![picture 0](https://oss.docs.z-xin.net/552b9a3270841d79860faa338a65e4892e641baa4989ff861a3b768bad7a306b.png)  

### 3.3 格式化文本

`python-docx`还允许对文本进行格式化，比如设置字体、颜色、大小和样式。

```py
from docx.shared import Pt
from docx.oxml.ns import qn
from docx.shared import RGBColor

# 创建一个段落
p = doc.add_paragraph()

# 添加文本
p.add_run('这是加粗的文本。').bold = True
p.add_run('这是斜体的文本。').italic = True

# 设置字体大小和颜色
run = p.add_run('这是红色的文本。')
run.font.size = Pt(14)
run.font.color.rgb = RGBColor(255,0,0)

# 添加下划线
run = p.add_run('这是带下划线的文本。')
run.underline = True

```

| 属性          | 描述                                   |
| ------------- | -------------------------------------- |
| bold          | 文本以粗体出现                         |
| italic        | 文本以斜体出现                         |
| underline     | 文本带下划线                           |
| strike        | 文本带删除线                           |
| double_strike | 文本带双删除线                         |
| all_caps      | 文本以大写首字母出现                   |
| small_caps    | 文本以大写首字母出现，小写字母小两个点 |
| shadow        | 文本带阴影                             |
| outline       | 文本以轮廓线出现，而不是实心           |
| rtl           | 文本从右至左书写                       |
| imprint       | 文本以刻入页面的方式出现               |
| emboss        | 文本以凸出页面的方式出现               |

### 3.4 插入表格（add_table)

```py
from docx.oxml.ns import qn
from docx.shared import Inches

# 创建一个表格
table = doc.add_table(rows=3, cols=3)

# 设置表格样式
table.style = 'Table Grid'

# 填充表格数据
for row in table.rows:
    for cell in row.cells:
        cell.text = '单元格内容'

# 合并单元格
table.cell(0, 0).merge(table.cell(1, 1))
```



### 3.5 插入图片

要插入图片，使用`add_picture`方法。确保图片文件存在于相应的路径

```python
from docx.shared import Inches

#插入图片
doc.add_picture('example.png', width=Inches(4), height=Inches(3))
```



### 3.6 保存文档

当完成文档的创建和编辑后，使用`save`方法将文档保存到磁盘

```
doc.save('mydoc.docx')
```



## 4.python-docx使用-读

### 4.1 读取文档

```python
# coding:utf-8

import os
from docx import Document

path = os.path.join(os.getcwd(), 'test_file/文本.docx')
print("\'文本.docx\' 的路径为：", path)     # 调试路径

doc = Document(path)

for p in doc.paragraphs:
    print(p.text)

```

### 4.2 读取文档中表格内容

```python
# coding:utf-8

import os
from docx import Document

path = os.path.join(os.getcwd(), 'test_file/文本.docx')
print("\'文本.docx\' 的路径为：", path)     # 调试路径

doc = Document(path)

# for p in doc.paragraphs:
#     print(p.text)

for t in doc.tables:            # for 循环获取表格对象
    for row in t.rows:          # 获取每一行
        row_str = []
        for cell in row.cells:    # 获取每一行单独的小表格,然后将其内容拼接起来;拼接完成之后再第二个for循环中打印出来
            row_str.append(cell.text)
        print(row_str)
        
# 也可以通过 "columns" 获取表格中的列的内容，可以自己尝试一下

```













## 参考：

https://zhuanlan.zhihu.com/p/667184141

