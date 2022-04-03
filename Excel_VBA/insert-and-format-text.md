#### Insert and Format Text

```python
Sub InsertTxt()

    Range("A1").Value = "Out Global Company"
    Range("A2").Value = "Stock Prices"
    Range("B4").Value = ActiveSheet.Name & " Portfolio"
    Range("B6").Value = "Symbol"
    Range("C6").Value = "Open"
    Range("D6").Value = "Close"
    Range("E6").Value = "Net Change"
    
End Sub
```

```python
Sub FormatTxt()

    Range("A1").Select
    With Selection.Font
        .Name = "Calibri"
        .Size = 20
        .Bold = True
    End With
    
    Range("A2").Select
    With Selection.Font
        .Name = "Calibri"
        .Size = 18
        .Bold = True
    End With
    
    Range("B4").Select
    With Selection.Font
        .Name = "Calibri"
        .Size = 14
        .Bold = True
    End With
    
    Range("B6:E6").Select
    With Selection.Font
        .Name = "Calibri"
        .Size = 12
        .Bold = Ture
    End With
   
    Columns("C:E").Select
    Selection.Style = "Currency"
    Columns("B:E").Select
    Columns("B:E").EntireColumn.AutoFit
    
    Range("E1").Select

End Sub
```

```python
Sub FormatList()

    Range("B6").CurrentRegion.Select
    Selection.Interior.ThemeColor = xlThemeColorDark2
    
    Range("E1").Select
    
End Sub
```

**Bofore**

<img width="157" alt="Screen Shot 2022-04-03 at 5 43 14 PM" src="https://user-images.githubusercontent.com/73784742/161421971-3c1c6d6c-d5dd-436e-a772-0cb7aaa5c64f.png">

**After**

<img width="354" alt="Screen Shot 2022-04-03 at 5 44 39 PM" src="https://user-images.githubusercontent.com/73784742/161421983-fd33978e-46ef-4d1a-aaf2-a3a12a8d09a5.png">
