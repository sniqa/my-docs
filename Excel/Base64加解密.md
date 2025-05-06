### 在Excel中进行Base64编码

要在Excel中进行Base64编码，可以按照以下步骤操作：
- **打开VBA编辑器**：按下_Alt + F11_打开VBA编辑器。
- **插入模块**：在插入菜单中选择“模块”。
- **复制并粘贴以下VBA代码**：
```vba
Function Encoding(text$)

Dim i

With CreateObject("ADODB.Stream")

.Open: .Type = 2: .Charset = "utf-8"

.WriteText text: .Position = 0: .Type = 1: i = .Read

With CreateObject("Microsoft.XMLDOM").createElement("b64")

.DataType = "bin.base64": .nodeTypedValue = i

Encoding = Replace(Mid(.text, 5), vbLf, "")

End With

.Close

End With

End Function
```

### 在Excel中进行Base64解码

要在Excel中进行Base64解码，可以按照以下步骤操作：
- **打开VBA编辑器**：按下_Alt + F11_打开VBA编辑器。
- **插入模块**：在插入菜单中选择“模块”。
- **复制并粘贴以下VBA代码**：
```
Function Decoding(b64$)

Dim i

With CreateObject("Microsoft.XMLDOM").createElement("b64")

.DataType = "bin.base64": .Text = b64

i = .nodeTypedValue

With CreateObject("ADODB.Stream")

.Open: .Type = 1: .Write i: .Position = 0: .Type = 2: .Charset = "utf-8"

Decoding = .ReadText

.Close

End With

End With

End Function
```