#### Sheet Template Generator and Reordering Sheets

```python
Sub RenameSheets()

Dim x As Integer

For x = 1 To 12

    Worksheets(1).Copy After:=Worksheets(x)
    Worksheets(x + 1).Name = Format(DateSerial(1, x, 1), "mmmm")
Next x

Worksheets(1).Delete

End Sub
```
---
```python
Sub SEMove()

' SEMove Macro
' Moves Southeast worksheets to beginning of worksheet list.

Worksheets("SE Sales").Move Before:=Worksheets(1)
End Sub
```
