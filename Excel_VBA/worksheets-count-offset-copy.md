#### Worksheets, Count, Offset, Copy 

```python
Public Sub YouTry()

Dim x As Integer
Dim sheet_title As String

For x = 1 To Worksheets.Count - 1

Worksheets(x).Select
sheet_title = ActiveSheet.Name
Worksheets("P&L").Select

Range("A1").Select
Selection.Offset(x * 5 + 2, 0).Select
Selection.Value = sheet_title
Selection.Font.Bold = True

Worksheets(x).Select
Range("A1").Select
Selection.CurrentRegion.Copy
Sheets("P&L").Select

ActiveCell.Offset(1, 0).Select
ActiveSheet.PasteSpecial

Next x

Rows("3:6").Delete

End Sub
```
