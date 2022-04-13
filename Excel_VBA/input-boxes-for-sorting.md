#### Input Boxes for Sorting

```python
Public Sub testMsgBox()

Dim Input_received As Integer

Input_received = MsgBox("Would you like to download the sunshine virus?", vbYesNo, "It's a beautiful day!")

If Input_received = vbYes Then
MsgBox "Go play in the sunshine!"
Else: MsgBox "Well don't work too hard"
End If

End Sub
```

```python
Public Sub testMsgBox()

Dim IntResponse As Integer

IntResponse = MsgBox("Would you like to download the sunshine virus?", _
vbYesNo, "It's a beautiful day!")

If IntResponse = vbYes Then
MsgBox "Wonderful!" & vbCrLf & "Go play in the sunshine!"

' vbCrLf -> hard return within a line of text

Else: MsgBox "Well don't work too hard"
End If

End Sub
```

```python

Public Sub testMsgBox()

Dim strResponse As String

strResponse = InputBox("What would you like to name this sheet?", "Name the sheet")

ActiveSheet.Name = strResponse

End Sub
```

```python
Public Sub testMsgBox()

Dim strResponse As String

strResponse = InputBox("Which dwarf is your favorite?")

Select Case strResponse
    Case "Grumpy"
        MsgBox "Cool"
    Case "Dopey"
        MsgBox "Wow"
    Case "Doc"
        MsgBox "Awesome"
    Case Else
        MsgBox "Hmm, Interesting!"
End Select

End Sub
```

---

```python
Sub DateThenTime()

' DateThenTime Macro
' Sorts table by date and then time.

    Range("A5:G78").Select
    Selection.Sort Key1:=Range("A5"), Order1:=xlAscending, Key2:=Range("B5") _
        , Order2:=xlAscending, Header:=xlGuess, OrderCustom:=1, MatchCase:= _
        False, Orientation:=xlTopToBottom, DataOption1:=xlSortNormal, DataOption2 _
        :=xlSortNormal
End Sub
```

```python
Sub RepSort()

' RepSort Macro
' Sorts by Rep, Date, and then Time
    
    Range("A5:G78").Select
    Selection.Sort Key1:=Range("C5"), Order1:=xlAscending, Key2:=Range("A5") _
        , Order2:=xlAscending, Key3:=Range("B5"), Order3:=xlAscending, Header:= _
        xlGuess, OrderCustom:=1, MatchCase:=False, Orientation:=xlTopToBottom, _
        DataOption1:=xlSortNormal, DataOption2:=xlSortNormal, DataOption3:= _
        xlSortNormal
End Sub
```

```python
Sub SortBy()

Dim Message, TitleBarTxt, DefaultTxt, SortVal As String
Dim YNAnswer As Integer

Message = "Enter a number to sort by the following fields:" & vbCrLf & _
" 1 - By Date and Time" & vbCrLf & _
" 2 - By Customer Service Rep, Date & Time"

TitleBarTxt = "Sort Call Center Log"
DefaultTxt = "Enter 1 or 2"

SortVal = InputBox(Message, TitleBarTxt, DefaultTxt)

Select Case SortVal
    Case "1"
        Call DateThenTime
    Case "2"
        Call RepSort
    Case Else
        YNAnswer = MsgBox("You didn't type 1 or 2. Try Again?", vbYesNo)
        If YNAnswer = vbYes Then
            Call SortBy
        End If
        ' Nothing happens
End Select
        
End Sub
```
