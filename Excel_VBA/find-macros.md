#### FIND Macros

```python
Sub SortLastCol()
    Range("A1").Select
    ActiveCell.CurrentRegion.Select
    
    Selection.Sort Key1:=Range("D1"), Order1:=xlDescending, Header:=xlGuess, _
        OrderCustom:=1, MatchCase:=False, Orientation:=xlTopToBottom, _
        DataOption1:=xlSortNormal
        
End Sub
```

```python
Sub InsColRow()

    Columns("A:A").Select
    Selection.Insert Shift:=xlToRight
    Rows("1:6").Select
    Selection.Insert Shift:=xlDown
    
End Sub
```

```python
Sub InsertTxt()

    Range("A1").Select
    ActiveCell.FormulaR1C1 = "Our Global Company"
    
    Range("A2").Select
    ActiveCell.FormulaR1C1 = "Stock Prices"
    
    Range("B4").Select
    Selection.Value = ActiveSheet.Name & " Portfolio"
    
    Range("B6").Select
    ActiveCell.FormulaR1C1 = "Symbol:"
    
    Range("C6").Select
    ActiveCell.FormulaR1C1 = "Open:"
        
    Range("D6").Select
    ActiveCell.FormulaR1C1 = "Close:"
    
    Range("E6").Select
    ActiveCell.FormulaR1C1 = "Net Chg:"
    
End Sub
```

```python
Sub FmtTxt()

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
    
    Range("B4").Select
    With Selection.Font
        .Name = "Arial"
        .Size = 14
        .Bold = True
    End With
    
    Range("B6:E6").Select
    With Selection.Font
        .Name = "Arial"
        .Size = 12
        .Bold = True
    End With
    
    Columns("B:E").Select
    Selection.Style = "Currency"
    
    Columns("B:J").Select
    Selection.Columns.AutoFit

End Sub
```

```python
Sub GenRep()

Dim x As Integer

For x = 1 To Worksheets.Count - 1

Worksheets(x).Select
Call SortLastCol
Call InsColRow
Call InsertTxt
Call FmtTxt

Range("B6").Select
Selection.CurrentRegion.Select
Selection.Copy
Worksheets("All Portfolios").Select
Range("B200").Select
Selection.End(xlUp).Select
ActiveCell.Offset(2, 0).Select
ActiveCell.Value = Worksheets(x).Name & " Protfolio"
ActiveCell.Offset(2, 0).Select
ActiveSheet.Paste

Next x

Call FindFormatPortHeaders

Columns("B:E").Select
Selection.Columns.AutoFit
    
End Sub
```

```python
Sub FindFormatPortHeaders()

Dim FoundCellAddress As String
Range("B4").Select
FoundCellAddress = ActiveCell.Address

Cells.Find(What:="portfolio", After:=ActiveCell, LookIn:=xlFormulas2, _
          LookAt:=xlPart, SearchOrder:=xlByRows, SearchDirection:=xlNext, _
          MatchCase:=False).Activate
        
        Do
           With Selection.Font
                .Name = "Arial"
                .Size = 14
                .Bold = True
           End With
           Cells.FindNext(After:=ActiveCell).Activate
        Loop While FoundCellAddress <> ActiveCell.Address

End Sub
```
