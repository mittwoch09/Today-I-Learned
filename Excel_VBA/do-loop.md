#### Do Loop

```python
Public Sub ExampleDoWhileLoop()

Dim x As Integer

x = 1

Do While x < 10
Cells(x, 1).Value = 100
x = x + 1
Loop

End Sub
```

```python
Public Sub ExampleDoWhileCalcLoop()

Dim x As Integer
x = 5

Do While Cells(x, 1).Value <> ""
' <> => NOT
Cells(x, 3).Value = Cells(x, 2).Value + 30
x = x + 1
Loop

End Sub
```

```python
Public Sub DoUntilLoopEx()

Dim intRow As Integer

intRow = 1

Do Until IsEmpty(Cells(intRow, 1))

Cells(intRow, 1).Value = "Info"
intRow = intRow + 1
Loop

End Sub
```

```python
Public Sub DoLoopUNTIL()

Dim intRow As Integer

intRow = 1

Do

Cells(intRow, 1).Value = "Info"
intRow = intRow + 1
Loop Until IsEmpty(Cells(intRow, 1))

End Sub
```
