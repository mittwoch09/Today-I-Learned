### DateSerial and Format

```python
Public Sub DatesinThisMonth()

Dim x, MonthValue, YearValue As Integer

MontValue = Range("B5").Value
YearValue = Range("C5").Value

For x = 1 To 30
Range("D" & 4 + x).Value = DateSerial(YearValue, MonthValue, x)

Next x

End Sub
```

```python
Public Sub FormatasLong()

Dim x As Integer
Dim DateValue As Date

For x = 1 To 30
DateValue = Range("D" & 4 + x)
Range("D" & 4 + x) = Format(DateValue, "Long Date")
' convert them into text values

Next x

End Sub
```
