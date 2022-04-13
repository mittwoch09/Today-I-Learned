#### Move, Copy and Print based on User Input

```python
Sub aMoveCopyPrintDivisionQuarter()

Call aMovebyInput
Call aCreateQuarterlyLog
Call aDuplicateMonthlyLog
Call aPrintTeamQuarter

End Sub
```

```python
Sub aMovebyInput()

' PrintDivision Macro

  Dim Message As String
  Dim Region, YNAnswer As Integer


  Message = "Enter the region of your choice:" & vbCrLf & _
            "1 - Southeast" & vbCrLf & _
            "2 - Northeast" & vbCrLf & _
            "3 - Mid-west" & vbCrLf & _
            "4 - Southwest" & vbCrLf & _
            "5 - Northwest" & vbCrLf & _
            "6 - Far-west"
  Region = InputBox(Message, "Region", "Enter 1, 2, 3, 4, 5, or 6")
  
Select Case Region
    Case 1
        Call SEMove
    Case 2
        Call NEMove
    Case 3
        Call MWMove
    Case 4
        Call SWMove
    Case 5
        Call NWMove
    Case 6
        Call FWMove
    Case Else
        YNAnswer = MsgBox("You didn't type a number between 1 and 6. Try Again?", vbYesNo)
        If YNAnswer = vbYes Then
        Call aMovebyInput
        End If
    End Select
  
End Sub
```

```python
Sub aCreateQuarterlyLog()

' Figure out where the names list stops

Range("A2000").End(xlUp).Select
Selection.Offset(3, 0).Select

Selection.Value = "LOG"
With Selection.Font
    .Size = 16
    .Bold = True
End With

Selection.Offset(1, 0).Select
Selection.Value = "Date"
Selection.Font.Bold = True

Selection.Offset(0, 1).Select
Selection.Value = "Client Name"
Selection.Font.Bold = True

Selection.Offset(0, 1).Select
Selection.Value = "Contact Name"
Selection.Font.Bold = True

Selection.Offset(0, 1).Select
Selection.Value = "Duration"
Selection.Font.Bold = True

Selection.Offset(0, 1).Select
Selection.Value = "Notes:"
Selection.Font.Bold = True

End Sub
```

```python
Sub aDuplicateMonthlyLog()

Dim QuarterSelect, YNAnswer, y As Integer
QuarterSelect = InputBox("Which Quarter is this for?")

Select Case QuarterSelect
    Case 1
        QuarterSelect = 1
    Case 2
        QuarterSelect = 4
    Case 3
        QuarterSelect = 7
    Case 4
        QuarterSelect = 10
    Case Else
        YNAnswer = MsgBox("You didn't enter a valid quarter number. Try Again?", vbYesNo)
        If YNAnswer = vbYes Then
            Call aDuplicateMonthlyLog
        End If
End Select
 
For y = 1 To 3

Worksheets(1).Copy After:=Worksheets(y)
Worksheets(y + 1).Name = Format(DateSerial(1, QuarterSelect + y - 1, 1), "MMMM")

Next y

End Sub
```

```python
Sub aPrintTeamQuarter()

Worksheets(2).PrintOut
Worksheets(3).PrintOut
Worksheets(4).PrintOut

End Sub
```
