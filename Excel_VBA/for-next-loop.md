#### For Next Loop

```python
Public Sub ForNext()

Dim x As Integer

For x = 1 To 10
Cells(x, 1).Value = 100
Next x

End Sub
```

```python
Public Sub DoubleForNext()

Dim intCol As Integer
Dim intRow As Integer

For intCol = 1 To 3
For intRow = 1 To 10
Cells(intRow, intCol).Value = "Wow!"
Next intRow
Next intCol

End Sub
```

```python
Public Sub TripleForNext()

Dim intCol As Integer
Dim intRow As Integer
Dim intSheet As Integer

For intSheet = 3 To 5
For intCol = 1 To 3
For intRow = 1 To 10

Worksheets(intSheet).Cells(intRow, intCol).Value = "Whoa!"

Next intRow
Next intCol
Next intSheet

'Have to be in opposite order!

End Sub
```

```python
Public Sub ExForEach()

Dim x As Worksheet

For Each x In Worksheets
MsgBox "Found Worksheet:" & x.Name
Next x

End Sub
```

```python
Public Sub ExitForExample()

Dim x As Integer

For x = 2 To 50
Range("B" & x).Select
If Range("B" & x).Value = "Stop" Then
    Exit For
ElseIf Range("B" & x).Value = "" Then
    Range("B" & x).Value = "Info"
End If

Next x

End Sub
```
