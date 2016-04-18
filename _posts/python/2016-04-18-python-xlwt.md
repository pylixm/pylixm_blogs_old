---
layout : post
title : python xlrd和xlwt 使用记录
category : python-lib
date : 2016-04-18
tags : [python, xlwt, xlrd ]
---

![TOC]

记录下xlrd和xlwt的使用demo :


### xlwt 

#### The Simplest Example

{% highlight python %}
import xlwt
workbook = xlwt.Workbook(encoding = 'ascii')
worksheet = workbook.add_sheet('My Worksheet')
worksheet.write(0, 0, label = 'Row 0, Column 0 Value')
workbook.save('Excel_Workbook.xls')
{% endhighlight %}

#### Formatting the Contents of a Cell

{% highlight python %}
import xlwt
workbook = xlwt.Workbook(encoding = 'ascii')
worksheet = workbook.add_sheet('My Worksheet')
font = xlwt.Font() # Create the Font
font.name = 'Times New Roman'
font.bold = True
font.underline = True
font.italic = True
style = xlwt.XFStyle() # Create the Style
style.font = font # Apply the Font to the Style
worksheet.write(0, 0, label = 'Unformatted value')
worksheet.write(1, 0, label = 'Formatted value', style) # Apply the Style to the Cell
workbook.save('Excel_Workbook.xls')
{% endhighlight %}

#### Attributes of the Font Object

{% highlight python %}
font.bold = True # May be: True, False
font.italic = True # May be: True, False
font.struck_out = True # May be: True, False
font.underline = xlwt.Font.UNDERLINE_SINGLE # May be: UNDERLINE_NONE, UNDERLINE_SINGLE, UNDERLINE_SINGLE_ACC, UNDERLINE_DOUBLE, UNDERLINE_DOUBLE_ACC
font.escapement = xlwt.Font.ESCAPEMENT_SUPERSCRIPT # May be: ESCAPEMENT_NONE, ESCAPEMENT_SUPERSCRIPT, ESCAPEMENT_SUBSCRIPT
font.family = xlwt.Font.FAMILY_ROMAN # May be: FAMILY_NONE, FAMILY_ROMAN, FAMILY_SWISS, FAMILY_MODERN, FAMILY_SCRIPT, FAMILY_DECORATIVE
font.charset = xlwt.Font.CHARSET_ANSI_LATIN # May be: CHARSET_ANSI_LATIN, CHARSET_SYS_DEFAULT, CHARSET_SYMBOL, CHARSET_APPLE_ROMAN, CHARSET_ANSI_JAP_SHIFT_JIS, CHARSET_ANSI_KOR_HANGUL, CHARSET_ANSI_KOR_JOHAB, CHARSET_ANSI_CHINESE_GBK, CHARSET_ANSI_CHINESE_BIG5, CHARSET_ANSI_GREEK, CHARSET_ANSI_TURKISH, CHARSET_ANSI_VIETNAMESE, CHARSET_ANSI_HEBREW, CHARSET_ANSI_ARABIC, CHARSET_ANSI_BALTIC, CHARSET_ANSI_CYRILLIC, CHARSET_ANSI_THAI, CHARSET_ANSI_LATIN_II, CHARSET_OEM_LATIN_I
font.colour_index = ?
font.get_biff_record = ?
font.height = 0x00C8 # C8 in Hex (in decimal) = 10 points in height.
font.name = ?
font.outline = ?
font.shadow = ?
{% endhighlight %}


#### Setting the Width of a Cell

{% highlight python %}
import xltw
workbook = xlwt.Workbook()
worksheet = workbook.add_sheet('My Sheet')
worksheet.write(0, 0, 'My Cell Contents')
worksheet.col(0).width = 3333 # 3333 = 1" (one inch).
workbook.save('Excel_Workbook.xls')
{% endhighlight %}

#### Entering a Date into a Cell

{% highlight python %}
import xlwt
import datetime
workbook = xlwt.Workbook()
worksheet = workbook.add_sheet('My Sheet')
style = xlwt.XFStyle()
style.num_format_str = 'M/D/YY' # Other options: D-MMM-YY, D-MMM, MMM-YY, h:mm, h:mm:ss, h:mm, h:mm:ss, M/D/YY h:mm, mm:ss, [h]:mm:ss, mm:ss.0
worksheet.write(0, 0, datetime.datetime.now(), style)
workbook.save('Excel_Workbook.xls')
{% endhighlight %}
    
#### Adding a Formula to a Cell

{% highlight python %}
import xlwt
workbook = xlwt.Workbook()
worksheet = workbook.add_sheet('My Sheet')
worksheet.write(0, 0, 5) # Outputs 5
worksheet.write(0, 1, 2) # Outputs 2
worksheet.write(1, 0, xlwt.Formula('A1*B1')) # Should output "10" (A1[5] * A2[2])
worksheet.write(1, 1, xlwt.Formula('SUM(A1,B1)')) # Should output "7" (A1[5] + A2[2])
workbook.save('Excel_Workbook.xls')
{% endhighlight %}

#### Adding a Hyperlink to a Cell

{% highlight python %}
import xlwt
workbook = xlwt.Workbook()
worksheet = workbook.add_sheet('My Sheet')
worksheet.write(0, 0, xlwt.Formula('HYPERLINK("http://www.google.com";"Google")')) # Outputs the text "Google" linking to http://www.google.com
workbook.save('Excel_Workbook.xls')
{% endhighlight %}

#### Merging Columns and Rows

{% highlight python %}
import xlwt
workbook = xlwt.Workbook()
worksheet = workbook.add_sheet('My Sheet')
worksheet.write_merge(0, 0, 0, 3, 'First Merge') # Merges row 0's columns 0 through 3.
font = xlwt.Font() # Create Font
font.bold = True # Set font to Bold
style = xlwt.XFStyle() # Create Style
style.font = font # Add Bold Font to Style
worksheet.write_merge(1, 2, 0, 3, 'Second Merge', style) # Merges row 1 through 2's columns 0 through 3.
workbook.save('Excel_Workbook.xls')
{% endhighlight %}

#### Setting the Alignment for the Contents of a Cell

{% highlight python %}
import xlwt
workbook = xlwt.Workbook()
worksheet = workbook.add_sheet('My Sheet')
alignment = xlwt.Alignment() # Create Alignment
alignment.horz = xlwt.Alignment.HORZ_CENTER # May be: HORZ_GENERAL, HORZ_LEFT, HORZ_CENTER, HORZ_RIGHT, HORZ_FILLED, HORZ_JUSTIFIED, HORZ_CENTER_ACROSS_SEL, HORZ_DISTRIBUTED
alignment.vert = xlwt.Alignment.VERT_CENTER # May be: VERT_TOP, VERT_CENTER, VERT_BOTTOM, VERT_JUSTIFIED, VERT_DISTRIBUTED
style = xlwt.XFStyle() # Create Style
style.alignment = alignment # Add Alignment to Style
worksheet.write(0, 0, 'Cell Contents', style)
workbook.save('Excel_Workbook.xls')
{% endhighlight %}

#### Adding Borders to a Cell

Please note: While I was able to find these constants within the source code, on my system (using LibreOffice,) I was only presented with a solid line, varying from thin to thick; no dotted or dashed lines.

{% highlight python %}
import xlwt
workbook = xlwt.Workbook()
worksheet = workbook.add_sheet('My Sheet')
borders = xlwt.Borders() # Create Borders
borders.left = xlwt.Borders.DASHED # May be: NO_LINE, THIN, MEDIUM, DASHED, DOTTED, THICK, DOUBLE, HAIR, MEDIUM_DASHED, THIN_DASH_DOTTED, MEDIUM_DASH_DOTTED, THIN_DASH_DOT_DOTTED, MEDIUM_DASH_DOT_DOTTED, SLANTED_MEDIUM_DASH_DOTTED, or 0x00 through 0x0D.
borders.right = xlwt.Borders.DASHED
borders.top = xlwt.Borders.DASHED
borders.bottom = xlwt.Borders.DASHED
borders.left_colour = 0x40
borders.right_colour = 0x40
borders.top_colour = 0x40
borders.bottom_colour = 0x40
style = xlwt.XFStyle() # Create Style
style.borders = borders # Add Borders to Style
worksheet.write(0, 0, 'Cell Contents', style)
workbook.save('Excel_Workbook.xls')
{% endhighlight %}


#### Setting the Background Color of a Cell

{% highlight python %}
import xlwt
workbook = xlwt.Workbook()
worksheet = workbook.add_sheet('My Sheet')
pattern = xlwt.Pattern() # Create the Pattern
pattern.pattern = xlwt.Pattern.SOLID_PATTERN # May be: NO_PATTERN, SOLID_PATTERN, or 0x00 through 0x12
pattern.pattern_fore_colour = 5 # May be: 8 through 63. 0 = Black, 1 = White, 2 = Red, 3 = Green, 4 = Blue, 5 = Yellow, 6 = Magenta, 7 = Cyan, 16 = Maroon, 17 = Dark Green, 18 = Dark Blue, 19 = Dark Yellow , almost brown), 20 = Dark Magenta, 21 = Teal, 22 = Light Gray, 23 = Dark Gray, the list goes on...
style = xlwt.XFStyle() # Create the Pattern
style.pattern = pattern # Add Pattern to Style
worksheet.write(0, 0, 'Cell Contents', style)
workbook.save('Excel_Workbook.xls')
{% endhighlight %}

TODO: Things Left to Document

- Panes -- separate views which are always in view

- Border Colors (documented above, but not taking effect as it should)

- Border Widths (document above, but not working as expected)

- Protection

- Row Styles

- Zoom / Manification

- WS Props?

Source Code for reference available at: https://secure.simplistix.co.uk/svn/xlwt/trunk/xlwt/

#### 另一种使用方式 

{% highlight python %}
import xlwt;

#styleBlueBkg = xlwt.easyxf('font: color-index red, bold on');
#styleBlueBkg = xlwt.easyxf('font: background-color-index red, bold on');
#styleBlueBkg = xlwt.easyxf('pattern: pattern solid, fore_colour red;');
#styleBlueBkg = xlwt.easyxf('pattern: pattern solid, fore_colour blue;');
#styleBlueBkg = xlwt.easyxf('pattern: pattern solid, fore_colour light_blue;');
#styleBlueBkg = xlwt.easyxf('pattern: pattern solid, fore_colour pale_blue;');
#styleBlueBkg = xlwt.easyxf('pattern: pattern solid, fore_colour dark_blue;');
#styleBlueBkg = xlwt.easyxf('pattern: pattern solid, fore_colour dark_blue_ega;');
#styleBlueBkg = xlwt.easyxf('pattern: pattern solid, fore_colour ice_blue;');
styleBlueBkg = xlwt.easyxf('pattern: pattern solid, fore_colour ocean_blue; font: bold on;'); # 80% like
#styleBlueBkg = xlwt.easyxf('pattern: pattern solid, fore_colour sky_blue;');
    
    
#blueBkgFontStyle = xlwt.XFStyle()
#blueBkgFontStyle.Pattern = blueBackgroundPattern;
#styleBlueBkg = blueBkgFontStyle;
    
styleBold   = xlwt.easyxf('font: bold on');
    
wb = xlwt.Workbook();
ws = wb.add_sheet('realPropertyInfo');
    
ws.write(0, 0, "Sequence",  styleBlueBkg);
ws.write(0, 1, "MapID",     styleBlueBkg);
ws.write(0, 2, "Owner1",    styleBold);
ws.write(0, 3, "Owner2",    styleBold);
    
wb.save(excelFilename);
{% endhighlight %}



### xlrd 

简单实例：

{% highlight python %}
#导入
import xlrd
#打开excel
data = xlrd.open_workbook('demo.xls') #注意这里的workbook首字母是小写
#查看文件中包含sheet的名称
data.sheet_names()
#得到第一个工作表，或者通过索引顺序 或 工作表名称
table = data.sheets()[0]
table = data.sheet_by_index(0)
table = data.sheet_by_name(u'Sheet1')
#获取行数和列数(实际含有数据的)
nrows = table.nrows
ncols = table.ncols
#获取整行和整列的值（数组）
table.row_values(i)
table.col_values(i)
#循环行,得到索引的列表
for rownum in range(table.nrows):
print table.row_values(rownum)
#单元格
cell_A1 = table.cell(0,0).value
cell_C4 = table.cell(2,3).value
#分别使用行列索引
cell_A1 = table.row(0)[0].value
cell_A2 = table.col(1)[0].value
#简单的写入
row = 0
col = 0
ctype = 1 # 类型 0 empty,1 string, 2 number, 3 date, 4 boolean, 5 error
value = 'lixiaoluo'
xf = 0 # 扩展的格式化 (默认是0)
table.put_cell(row, col, ctype, value, xf)
table.cell(0,0) # 文本:u'lixiaoluo'
table.cell(0,0).value # 'lixiaoluo'
{% endhighlight %}


--- 

### 参考

[http://www.crifan.com/python_xlwt_set_cell_background_color/](http://www.crifan.com/python_xlwt_set_cell_background_color/)
[http://blog.sina.com.cn/s/blog_5357c0af01019gjo.html](http://blog.sina.com.cn/s/blog_5357c0af01019gjo.html)