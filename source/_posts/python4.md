---
title: python处理excel文件
date: 2020-06-22 14:29:36
categories: python
tags: python excel
---

## 前提
python3.8
win10 64
vscode


## 操作excel
> 说明：整个excel文件称为工作簿，工作簿中的每个页称为工作表，工作表又由单元格组成

1. xlrd模块读取excel文件(xlrd模块既可读取xls文件也可读取xlsx文件)
1. xlwt模块写入excel文件(xlwt模块只能写xls文件，不能写xlsx文件)

```sh
# 检查模块是否安装
pip show xlrd
pip show xlwt

# 安装模块
pip install xlrd
pip install xlwt
```

### xlrd模块读取excel文件
年报excel（annals.xls）内容如下：
 
年份 |	季度|	营销额	|增长
---- |---- |---- |---- 
2016|	一季度|	2000|	10.20%
2017|	二季度|	3000|	34.00%
2018|	三季度|	2877|	28.30%
2019|	四季度|	3882|	42.00%

#### **xlrd怎么用**
```python
# 引入模块 
import xlrd
# 获取工作簿 
annals = xlrd.open_workbook('excel文件名.xls')
# 获取所有工作表名称,结果为列表 
names = annals.sheet_names()
# 根据索引或者名称获取工作表对象
# 索引
sheet = annals.sheet_by_index(i)
# 名称
sheet = annals.sheet_by_name('工作表名称')
# 获取工作表行数、列数
# 行数
rows = sheet.nrows
# 列数
cols = sheet.ncols
# 获取工作表某一行的内容,结果为列表
row = sheet.row_values(i)
# 获取工作表某一列的内容,结果为列表
col = sheet.col_values(i)
# 获取工作表某一单元格的的内容,结果为字符串或数值
cell = sheet.cell_value(m,n).value
```

#### **读取年报（annals.xls）**

```python
import xlrd
from logger import Loggings
loggings = Loggings()
annals = xlrd.open_workbook('annals.xls')
loggings.info(f'所有工作表名称：{annals.sheet_names()}')
sheet = annals.sheet_by_name('Sheel')
rows = sheet.nrows
cols = sheet.ncols
loggings.info(f'行：{rows}')
loggings.info(f'列：{cols}')
```

#### **xlwt怎么用**
```python
# 引入模块
import xlwt
# 创建工作簿,如果写入中文为乱码，可添加参数encoding = 'utf-8'
book = wlwt.Wortbook()
# 创建工作表
sheet = book.add_sheet('工作表名称')
# 向单元格写入内容
sheet.write(行下标,列下标,值)
# 保存工作簿，默认保存在py文件相同路径下，如果该路径下有相同文件，会被新创建的文件覆盖
book.save('excel文件名称')
```

#### **写入数据**
1. 逐步插入数据
```python
import xlwt
book = xlwt.Workbook()
sheet = book.add_sheet('Sheel')
sheet.write(0,0,'类型')
sheet.write(0,1,'书名')
book.save('book.xls')
```
book.xls内容如下：
类型|内容
--- | ---

1. 按行插入
```python
import xlwt
book = xlwt.Workbook()
sheet = book.add_sheet('Sheel')

titles = ['类型','书名']
types = ['java','算法']
booknames = ['think in java','计算机算法3']

for i in range(0,len(titles)):
  sheet.write(0,i,titles[i])
for i in range(0,len(types)):
  sheet.write(i+1,0,types[i])
for i in range(0,len(booknames)):
  sheet.write(i+1,1,booknames[i])
book.save('book.xls')
```
book.xls内容如下：
类型 | 内容
--- | ---
java | think in java
算法 | 计算机算法3

#### **自定义字体样式的excel表**
```python
import xlwt
book = xlwt.Workbook(encoding='utf-8')
sheet = book.add_sheet('shee1')
style = xlwt.XFStyle()  # 初始化样式
font = xlwt.Font()  # 初始化字体
font.name = 'Times New Roman'  # 使用字体的名称
font.bold = True  # 字体加粗
font.underline = True  # 字体加下划线
font.italic = True  # 斜体字
style.font = font  # 设定样式使用的字体
sheet.write(0,0, 'unformated')  # 不带样式的表单内容
sheet.write(0,1, 'formated', style)  # 带样式的表单内容
book.save('book.xls')
```

#### **设置单元格的宽度**
```python
book = xlwt.Workbook(encoding='utf-8')
sheet = book.add_sheet('shee1')
sheet.write(0, 0, '测试用例')
sheet.col(0).width = 3333  # 设置表单：sheet1 的第一列所有单元格的宽度
book.save('book.xls')
```

#### **输入一个日期到单元格**
```python
import datetime
book = xlwt.Workbook()
sheet = book.add_sheet('sheet1')
style = xlwt.XFStyle()  # 凡是设置表格属性，都需要初始化一个样式
style.num_format_str = 'M/D/YY'  # 设置当前表格的日期格式，以下为其他可选的格式
# D: 表示日期， M：表示月份，Y：表示年，h：表示小时，m：表示分钟，s：表示秒
# Other options: D-MMM-YY, D-MMM, MMM-YY, h:mm, h:mm:ss, h:mm, h:mm:ss, M/D/YY h:mm, mm:ss, [h]:mm:ss, mm:ss.0
sheet.col(1).width = 3333  # 设定日期表格宽度
sheet.write(0,1, datetime.datetime.today(), style)  # 设定当前表格日期采用style样式显示
book.save('book.xls')
```

#### **向表格添加一个公式**
```python
book = xlwt.Workbook()
sheet = book.add_sheet('sheet')
sheet.write(0, 0, 5)
sheet.write(0, 1, 2)
sheet.write(1, 0, xlwt.Formula(('A1*B1')))  # 实现第一行第一个字段和第二个字段值相乘，写入到第二行第一个表格内
sheet.write(1, 1, xlwt.Formula('SUM(A1,B1)'))  # 实现第一行第一个字段和第二个字段值相加，写入到第二行第二个表格内
book.save('book.xls')
```

#### **向一个表格添加一个超链接**
```python
book = xlwt.Workbook()
sheet = book.add_sheet('sheet_link')
sheet.write(0, 1, xlwt.Formula('HYPERLINK("http://www.baidu.com";"baidu")'))  # 在表格里创建一个超链接，名称为：baidu
book.save('book.xls')
```

#### **合并列和行**
```python
# 关于write_merge(x,m,y,n)参数说明：x 表示行数，m表示跨行个数， y表示列， n表示跨列个数，行和列的开始计数都为0
book = xlwt.Workbook()
sheet = book.add_sheet('sheet')
sheet.write_merge(0, 0, 1, 3, 'First merge')  
font = xlwt.Font()
font.bold = True
style = xlwt.XFStyle()
style.font = font
sheet.write_merge(1, 2, 0, 3, 'second merge', style)
book.save('book.xls')
```

#### **设置单元格内容的对其方式**
```python
workbook = xlwt.Workbook()
worksheet = workbook.add_sheet('My Sheet')
alignment = xlwt.Alignment() # Create Alignment
alignment.horz = xlwt.Alignment.HORZ_CENTER # May be: HORZ_GENERAL, HORZ_LEFT, HORZ_CENTER, HORZ_RIGHT, HORZ_FILLED, HORZ_JUSTIFIED, HORZ_CENTER_ACROSS_SEL, HORZ_DISTRIBUTED
alignment.vert = xlwt.Alignment.VERT_CENTER # May be: VERT_TOP, VERT_CENTER, VERT_BOTTOM, VERT_JUSTIFIED, VERT_DISTRIBUTED
style = xlwt.XFStyle() # Create Style
style.alignment = alignment # Add Alignment to Style
worksheet.write(0, 0, 'Cell Contents', style)
workbook.save('book.xls')
```

#### **为单元格设置背景色**
```python
book = xlwt.Workbook()
sheet = book.add_sheet('sheet')
pattern = xlwt.Pattern()  # 初始化一个图案
pattern.pattern = xlwt.Pattern.SOLID_PATTERN  # 可选：NO_PATTERN, SOLID_PATTERN, or 0x00 through 0x12
pattern.pattern_fore_colour = 5  # 背景颜色为黄色 
# 可选： 0 = Black, 1 = White, 2 = Red, 3 = Green, 4 = Blue, 5 = Yellow, 6 = Magenta, 7 = Cyan, 16 = Maroon, 17 = Dark Green, 18 = Dark Blue, 19 = Dark Yellow , almost brown), 20 = Dark Magenta, 21 = Teal, 22 = Light Gray, 23 = Dark Gray, the list goes on...
style = xlwt.XFStyle()
style.pattern = pattern  # 添加样式
sheet.write(0, 0, 'Cell content', style)
book.save('book.xls')
```

#### **处理xlsx文件**
openpyxl模块可实现对excel文件的读、写和修改，只能处理xlsx文件，不能处理xls文件
1. openpyxl读取excel文件
```python
# 引入模块
import openpyxl
# 获取工作簿对象
book = openpyxl.load_workbook('excel文件名称')
# 获取所有工作表名称
names = book.sheetnames
# 获取工作表对象
sheet = book.worksheets[i]
sheet2 = book['工作表名称']
sheet3 = book[book.sheetnames[i]]
# 获取工作表名称
title = sheet1.title
# 获取工作表行数
rows = sheet1.max_row
# 获取工作表列数
cols = sheet1.max_column
# 获取某一单元格内容，x/y 例如： 0,0  1,2 ，单元格 A1 B1
cell = sheet.cell(x,y).value
cell = sheet['单元格'].value
```

1. openpyxl写excel文件 
```python
# 引入模块
import openpyxl
# 创建工作簿
book = openpyxl.Workbook()
# 创建工作表，0表示创建的工作表在工作薄最前面
sheet = book.create_sheet('工作表名称',0)
# 向单元格写入内容 
sheet.cell(x,y,'内容1')
# 保存工作簿
book.save('excel文件名称')
```

1. openpyxl修改excel文件
```python
# 引入模块
import openpyxl
# 获取工作簿对象
book = openpyxl.load_workbook('excel文件名称')
# 获取所有工作表名称
names = book.sheetnames
# 获取工作表对象
sheet = book.worksheets[i]
# 在某行前插入
sheet.insert_rows(x)
# 在某列前插入
sheet.insert_cols(y)
# 删除指定行
sheet.delete_rows(x)
# 删除指定列
sheet.delete_cols(y)
# 修改单元格内容
sheet.cell(x,y) = '内容'
sheet['单元格'] = '内容'
# 在最后追加行
sheet.append([x,y,'内容'])
```


## 参考
* [python操作excel](http://www.python-excel.org/)
* [xlrd](https://github.com/python-excel/xlrd)
* [xlrd api](https://xlrd.readthedocs.io/en/latest/api.html)
* [openpyxl api](https://openpyxl.readthedocs.io/en/stable/)

