# Xlwings / glob
## xlwings
파이썬에서 엑셀과 상호작용할 수 있게 해주는 라이브러리

엑셀 프로그램 자체를 직접적으로 제어가능


## glob()
파일들의 리스트를 뽑을 때 사용


## excel.py
```python
# pip install xlwings
# xlwings addin install 
import xlwings as xw
import glob
# 특정파일명으로 시작 종료하는 파일들 확인하기 좋음

# 디렉토리와 확장자 입력
def 읽어들일파일들확인(dir, ext):
    file_list = glob.glob(f'{dir}/*.{ext}')
    file_list = [file for file in file_list if file.endswith(".xlsx") if not "-$" in file]
    
    return file_list

class Xlwork:    
    def __init__(self, file_full_path, sheet_num):
        #매크로 파일 설정
        xw.Book(file_full_path).set_mock_caller()
    
        #xlwings 통해 Workbook 호출
        self.wb = xw.Book.caller()
        
        #Sheet 설정
        self.sheet = self.wb.sheets[sheet_num]
        
        # self.last_num_row = self.sheet.range('A1').end('down').row
        # self.last_num_col = self.sheet.range('A1').end('right').column
        # self.last_num_row = self.sheet.range('A2').end('down').row
        # self.last_num_col = self.sheet.range('B1').end('right').column
    
    def 라스트넘버셋(self, row, col):
        self.last_num_row = self.sheet.range(row).end('down').row
        self.last_num_col = self.sheet.range(col).end('right').column
        return [self.last_num_row, self.last_num_col]
        
        
    def 셀의값변경(self, cell_name, value):    
        self.sheet[cell_name].value = value
        
    def 셀의값변경2(self, row_num, column_num, value):    
        self.sheet[row_num, column_num].value = value
        
    def 셀의값을가져오기(self, cell_name):
        return self.sheet[cell_name].value()
    
    def 셀의값을가져오기2(self, row_num, column_num):
        return self.sheet[row_num, column_num].value
    
    def 시트개수(self):
        return self.wb.sheets.count
    
    def 시트전환(self, sheet_num):
        self.sheet = self.wb.sheets[sheet_num]
        
    def 시트생성(self):
        self.wb.sheets.add()

```

## runner_excel.py
```python
from helpers.excel import Xlwork, 읽어들일파일들확인
import xlwings as xw
import pandas as pd

"""
각 엑셀 파일들을 읽어와서
각각의 시트들을 병합하고
그내용을 단일파일로 저장
결과물.xlsx
"""

if __name__ == "__main__":
    파일경로 = r"C:\workspace2\\pythonProject\\excel_ex\\결과엑셀 copy.xlsx"
    파일들 = 읽어들일파일들확인(r"C:\workspace2\\pythonProject\\excel_ex", "xlsx")
    
           
    result = Xlwork(r"C:\workspace2\\pythonProject\\helpers\\결과물.xlsx", 0)
    xlwork01=Xlwork(파일들[0], 0) 
    
       
    total_row = 0
    last_num = xlwork01.라스트넘버셋("A1", "A1")
     
    for i in range(0, last_num[0]):
        for ii in range(0, last_num[1]):
            value = xlwork01.셀의값을가져오기2(i, ii)
            print(xlwork01.셀의값을가져오기2(i, ii))
            result.셀의값변경2(total_row, ii, value)
            
        total_row += 1 
            
    result.시트생성()
    result.시트전환(0)
                
    
    xlwork01=Xlwork(파일들[1], 0) 
    total_row = 0
    last_num = xlwork01.라스트넘버셋("A2", "B1")
    
    for i in range(0, last_num[0]):
        for ii in range(0, last_num[1]):
            value = xlwork01.셀의값을가져오기2(i, ii)
            print(xlwork01.셀의값을가져오기2(i, ii))
            result.셀의값변경2(total_row,ii, value)
        
        total_row += 1

```
