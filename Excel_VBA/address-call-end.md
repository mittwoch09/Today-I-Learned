#### Adress, Call, End

```python
Public Sub AllMacros()

Call FindData
Call CopyPasteinA4
Call InsHeaders

End Sub
```

```python
Sub FindData()
'the following code will find the data, and remember where it is

Dim datastart As String

Range("A1").Select
Selection.End(xlDown).Select
datastart = ActiveCell.Address
Range(datastart).Select

End Sub
```

```python
Sub CopyPasteinA4()

Selection.CurrentRegion.Select
Selection.Cut
Range("A4").Select
ActiveSheet.Paste

End Sub
```

```python
Sub InsHeaders()

Range("A1").Select
Selection.Value = "Our Global Company"
Selection.Font.Bold = True
Selection.Font.Size = 16

Range("A3").Value = "Symbol"
Range("B3").Value = "Open"
Range("C3").Value = "Close"
Range("D3").Value = "Net Change"
Range("A3:D3").Font.Bold = True
Range("A3:D3").Font.Size = 12

Columns("A:D").AutoFit

End Sub
```
