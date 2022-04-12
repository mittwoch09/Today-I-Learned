#### Simple Variables

```python
Sub NumberVariable()

'Declare a Integer Variable
Dim aNumber As Integer

' Give it a Value
aNumber = 2

' Then use that value later on
Range("A5").Value = aNumber

End Sub
```

```python
Sub DateVariable()

'Use this to get VB to retrieve information and use it!

'Decalre a Integer Variable
Dim aDate As Date

'Give it a value from the sheet!
aDate = Range("E1").Value

' Then use it
Range("A5").Value = aDate

End Sub
```


```python
Sub StringVariable()

'Declare a String variable, name it "aProduct"
Dim aProduct As String

'Have it store information from cell G3
aProduct = Range("G3").Value

'Now place that content in cell B5
Range("B5").Value = aProduct

End Sub
```

```python
Sub NumberaVariable()

'Declare a Integer Variable, name it "aPrice"
Dim aPrice As Integer

'Have it store the prce information from H3
aPrice = Range("H3").Value

'Paste the price information in C5
Range("C5").Value = aPrice

End Sub
```

```python
Sub IncreasingNumber()

'Set variables to automatically increase their value!

'Declare an integer variable here
Dim IncreasingNumber As Integer

'Make it store information from the previous variable plus 1
IncreasingNumber = aPrice + 1

'Place this value in cell D5
Range("D5").Value = IncreasingNumber

End Sub
```

```python
Sub RunAllMacros()

'Run them all with a call statement
Call DateVariable
Call StringVariable
Call NumberaVariable
Call IncreasingNumber

End Sub
```
