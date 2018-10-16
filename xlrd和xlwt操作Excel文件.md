# xlrd和xlwt操作Excel文件
xlrd和xlwt是python的第三方库，所以是需要自己安装的。

```
pip install xlrd
pip install xlwt
```

## xlrd使用

- 导入模块
```
 import xlrd  
```
- 打开Excel文件读取数据
```
data = xlrd.open_workbook('excelFile.xls')  
```
- 使用技巧
	- 获取一个工作表
	```
        获取一个工作表
        table = data.sheets()[0]          #通过索引顺序获取
        table = data.sheet_by_index(0) #通过索引顺序获取
        table = data.sheet_by_name(u'Sheet1')#通过名称获取
	```
	- 获取整行和整列的值（数组)  
	```
        table.row_values(i)
        table.col_values(i)
	```
	- 获取行数和列数　
	```
        nrows = table.nrows 
        ncols = table.ncols
	```     
	-  	循环行列表数据 
 	```
        for i in range(nrows):
               print table.row_values(i)
	```
	- 单元格
	```
        cell_A1 = table.cell(0,0).value
        cell_C4 = table.cell(2,3).value
	```
	- 使用行列索引
	```
        cell_A1 = table.row(0)[0].value
        cell_A2 = table.col(1)[0].value
	```
	- 简单的写入
	```
        row = 0
        col = 0
        # 类型 0 empty,1 string, 2 number, 3 date, 4 boolean, 5 error
        ctype = 1 value = '单元格的值'
        xf = 0 # 扩展的格式化
        table.put_cell(row, col, ctype, value, xf)
        table.cell(0,0)  #单元格的值'
        table.cell(0,0).value #单元格的值'
	```

- xlrd用例
```
	import os  
	import xlrd  
	from datetime import date,datetime  
	  
	#打开Excel文件  
	workbook = xlrd.open_workbook('09-10.11-38-12-HTTP-GOOD-1-Lte1sDataStat_Charts.xlsx')  
	  
	#输出Excel文件中所有sheet的名字  
	print workbook.sheet_names()  
	  
	#根据sheet索引或者名称获取sheet内容  
	Data_sheet    = workbook.sheets()[0]  
	CdfData_sheet = workbook.sheet_by_index(1)  
	Charts_sheet  = workbook.sheet_by_name(u'Charts')  
	  
	#获取sheet名称、行数和列数  
	print Data_sheet.name,    Data_sheet.nrows,    Data_sheet.ncols,\  
	      CdfData_sheet.name, CdfData_sheet.nrows, CdfData_sheet.ncols,\  
	      Charts_sheet.name,  Charts_sheet.nrows,  Charts_sheet.ncols  
	  
	#获取整行和整列的值（列表）      
	rows = Data_sheet.row_values(0) #获取第一行内容  
	cols = Data_sheet.col_values(1) #获取第二列内容  
	#print rows  
	#print cols  
	  
	#获取单元格内容  
	cell_A1 = Data_sheet.cell(0,0).value  
	cell_C1 = Data_sheet.cell(0,2).value  
	cell_B1 = Data_sheet.row(0)[1].value  
	cell_D2 = Data_sheet.col(3)[1].value  
	print cell_A1, cell_B1, cell_C1, cell_D2  
	  
	#获取单元格内容的数据类型  
	#ctype:0 empty,1 string, 2 number, 3 date, 4 boolean, 5 error  
	print 'cell(0,0)数据类型:', Data_sheet.cell(0,0).ctype  
	print 'cell(1,0)数据类型:', Data_sheet.cell(1,0).ctype  
	print 'cell(1,1)数据类型:', Data_sheet.cell(1,1).ctype  
	print 'cell(1,2)数据类型:', Data_sheet.cell(1,2).ctype  
	  
	#获取单元格内容为日期的数据  
	date_value = xlrd.xldate_as_tuple(Data_sheet.cell_value(1,0),workbook.datemode)  
	print date_value  
	print '%d:%d:%d' %(date_value[3:])  
	  
	d = {'11:25:59':[1, 2, 3], '11:26:00':[2, 3, 4], '11:26:01':[3, 4, 5]}  
	print d['11:25:59']  
	print d['11:26:00']  
	print d['11:26:01']  
	  
	print d['11:25:59'][0]  
	print d['11:26:00'][0]  
	print d['11:26:01'][0]  
```

## xlwt使用

- 导入模块
```
from xlwt import *
```
- 初始化
```
w =Workbook()
```
Workbook类初始化时有encoding和style_compression参数。  
encoding，设置字符编码，一般要这样设置：w = Workbook(encoding='utf-8')，就可以在excel中输出中文了。默认是ascii。当然要记得在文件头部添加：`# -*- coding: utf-8-*-`
- 添加sheet
```
ws = w.add_sheet('xlwtwas here')
```
- 保存文件
```
w.save('mini.xls') 
```

## xlwt用例
```
import os  
import xlwt           
  
  
def set_style(name, height, bold = False):  
    style = xlwt.XFStyle()   #初始化样式  
      
    font = xlwt.Font()       #为样式创建字体  
    font.name = name  
    font.bold = bold  
    font.color_index = 4  
    font.height = height  
      
    style.font = font  
    return style  
  
      
def write_excel():  
    #创建工作簿  
    workbook = xlwt.Workbook(encoding='utf-8')    
    #创建sheet  
    data_sheet = workbook.add_sheet('demo')    
    row0 = [u'字段名称', u'大致时段', 'CRNTI', 'CELL-ID']  
    row1 = [u'测试', '15:50:33-15:52:14', 22706, 4190202]  
      
    #生成第一行和第二行  
    for i in range(len(row0)):  
        data_sheet.write(0, i, row0[i], set_style('Times New Roman', 220, True))  
        data_sheet.write(1, i, row1[i], set_style('Times New Roman', 220, True))  
      
    #保存文件  
    workbook.save('demo.xls')     
      
      
if __name__ == '__main__':   
    write_excel()  
    print u'创建demo.xlsx文件成功'  
```





	
