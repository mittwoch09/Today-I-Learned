#### Creating Macros from Scratch

```python
Public Sub FormatDataTest()

' 1. insert a Row at Top
' 2. Insert Headers for each column
' 3. Turn Row 1 Bold
' 4. Deselect

Rows("1:1").Insert

Range("A1").Value = "Emp ID"
Range("B1").Value = "Last Name"
Range("C1").Value = "First Name"

Rows("1:1").Font.Bold = True

Range("A1").Select

End Sub
```

---
**Range**

- Range("C3")
- Range("B2").Range("B2")
- Cells(3,3)
- [C3]
- Range("A1").Offset(2,2)
- 

  ```python
MyRange = "C3"
Range(MyRange)
  ```

**Color**

- Range("A1").Font.ThemeColor = xlThemeColorLight1
- Range("A1").Font.Color = vbRed
- Range("A1").Font.Color = RGB(1,1,1)
- Range("A1").Font.Color = 65535

```python
Range("C3").select
Selection.Interior.Color = vbBlue
```

**Rename the sheet**

```python
Sheets(1).Select
ActiveSheet.Name = "Portfolio 1"
```
