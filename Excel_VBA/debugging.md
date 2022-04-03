#### Debugging

```python
Sub CreateReport()

'Call to the macro that inserts columns and rows.

InsertRowsCols

'Call to the macro that inserts report headers.

InsertTxt

'Call to the macro that formats the report text.

FmtTxt

End Sub

Sub InsertRowsCols()

' InsertRowsCols Macro
' Inserting one column and three rows
' from the upper left corner of the worksheet.

    Rows("1:4").Select
    Selection.Insert Shift:=xlDown
    Columns("A:A").Select
    Selection.Insert Shift:=xlToRight


End Sub
```

```python
Sub InsertTxt()

' Inserting report title and column header text.

    Range("A1").Select
    
    ActiveCell.FormulaR1C1 = "Our Global Company"
    
    Range("A2").Select
    
    ActiveCell.FormulaR1C1 = "Stock Prices"
    
    Range("B4").Select
    
    ActiveCell.FormulaR1C1 = "Symbol:"
    
    Range("C4").Select
    
    ActiveCell.FormulaR1C1 = "Open:"
    
    Range("D4").Select
    
    ActiveCell.FormulaR1C1 = "High:"
    
    Range("E4").Select

    ActiveCell.FormulaR1C1 = "Low:"
    
    Range("F4").Select

    ActiveCell.FormulaR1C1 = "Close:"
    
    Range("G4").Select

    ActiveCell.FormulaR1C1 = "Net Chg:"
    
    Range("H4").Select

    ActiveCell.FormulaR1C1 = "Pct Chg:"
    
    Range("I4").Select

    ActiveCell.FormulaR1C1 = "Prt Alloc:"
    
    Range("J4").Select

    ActiveCell.FormulaR1C1 = "Prt Pct Chg:"
    
End Sub
```

```python
Sub FmtTxt()

' Formats inserted text and price and percent data
' in currency and percent format, then fits formatted
' data by auto-fitting columns.

    Columns("C:G").Select
    Selection.Style = "Currency"
    
    Columns("H:J").Select
    Selection.Style = "Percent"
    
    Range("A1").Select
    With Selection.Font
        .Name = "Arial"
        .Size = 18
        .Bold = True
    End With

    Range("A2").Select
    With Selection.Font
        .Name = "Arial"
        .Size = 14
        .Bold = True
    End With
    
    Range("B4:J4").Select
    With Selection.Font
        .Name = "Arial"
        .Size = 12
        .Bold = True
    End With
    
    Columns("B:J").EntireColumn.AutoFit
    
End Sub
```

**Before**

<img width="614" alt="Screen Shot 2022-04-03 at 3 42 39 PM" src="https://user-images.githubusercontent.com/73784742/161421924-182ad7b1-8ef5-4e40-879b-dccfa897370c.png">


**After**

<img width="565" alt="Screen Shot 2022-04-03 at 3 43 01 PM" src="https://user-images.githubusercontent.com/73784742/161421952-149469e1-85ce-427c-b5a1-b5fb7e878c94.png">
