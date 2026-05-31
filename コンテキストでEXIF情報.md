その昔、GITHUBなるサイトに、忘備録として描き残してたサイト存在を思い出すも、内容が不十分で再構築が叶わなかった。

そこで、今回、GoogleAIStudioの教示を元に、再挑戦してみた。

まずは･･･

① ツール「exiftool.exe」のダウンロード https://exiftool.org/から、64-bit: exiftool-13.58_64.zip をダウンロード 解凍後、C:\Tools\exiftool_filesとC:\Tools\exiftool.exeに移動

②次の内容を、「add_exif_context.reg」として、C:\Tools に保存する

```
Windows Registry Editor Version 5.00
[HKEY_CLASSES_ROOT\*\shell\ShowEXIF]
@="EXIFを表示（ExifTool）"
"Icon"="C:\\Tools\\exiftool.exe"
[HKEY_CLASSES_ROOT\*\shell\ShowEXIF\command]
@="powershell -NoExit -Command \"& 'C:\\Tools\\exiftool.exe' '%1'\""
```

③ add_exif_context.reg をＷクリックで、レジストリへ書き込まれる
※事前にregrditバックアップをする事

そうすると、右クリックコンテキ内に「ShowEXIF」が出現するが、現行のWindows11エクスプローラー表示形式では、「その他のオプションを確認」で開く二枚目のコンテキ内に出現する ⇒ コレ使いにくい‼️

④ コレ回避するには、エクスプローラーを「クラシックメニューに戻す設定」をすると最初のコンテキに出現する。 名前を classic_menu.reg などにして保存し、実行する。

```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32]
@=""
```

⑤ 「Windows 11の新しいデザイン」に戻すには、以下のコマンドをコマンドプロンプトで実行
```
reg delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f
```
以上

画像
<img width="1852" height="1468" alt="スクリーンショット 2026-05-26 212011" src="https://github.com/user-attachments/assets/982b6fa6-4390-452f-bd09-2404fd4c7a36" />

これで「ShowEXIF」機能は宜しいのだが、最近、使用頻度の高い「VSCode」コンテキに問題発生‼️

「クラシックメニュー設定」で、コンテキから消えた･･･「VSCode」

先に「VSCode」インストールパスを確認しておく事 ⇒

"C:\Users\kawa\AppData\Local\Programs\Microsoft VS Code\Code.exe"

① ホルダー単位 ⇒ コンテキ出現は
⇒ vscode_folder.reg を作ってRegedit書き込みする。
```
Windows Registry Editor Version 5.00
; フォルダを右クリックした時にVSCodeを出す設定
[HKEY_CLASSES_ROOT\Directory\shell\VSCode]
@="Open with Code"
"Icon"="\"C:\\Users\\%USERNAME%\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\""
[HKEY_CLASSES_ROOT\Directory\shell\VSCode\command]
@="\"C:\\Users\\%USERNAME%\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%1\""
; フォルダ内の何もない背景を右クリックした時にVSCodeを出す設定
[HKEY_CLASSES_ROOT\Directory\Background\shell\VSCode]
@="Open with Code"
"Icon"="\"C:\\Users\\%USERNAME%\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\""

[HKEY_CLASSES_ROOT\Directory\Background\shell\VSCode\command]
@="\"C:\\Users\\%USERNAME%\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%V\""
```
② ファイル単位 ⇒ コンテキ出現は
⇒ vscode_files.reg を作ってRegedit書き込みする。
```
Windows Registry Editor Version 5.00

; すべてのファイル(*)を右クリックした時にVSCodeを出す設定
[HKEY_CLASSES_ROOT\*\shell\VSCode]
@="Open with Code"
"Icon"="\"C:\\Users\\kawa\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\""

[HKEY_CLASSES_ROOT\*\shell\VSCode\command]
@="\"C:\\Users\\kawa\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%1\""

```


画像
【ホルダー単位】
<img width="445" height="583" alt="スクリーンショット 2026-05-26 233441" src="https://github.com/user-attachments/assets/c3f2212a-4914-4a8d-9f26-1757847b9ca9" />

画像
【ファイル単位】
<img width="516" height="635" alt="スクリーンショット 2026-05-27 064839" src="https://github.com/user-attachments/assets/2a26f18a-8767-4f5f-b65e-afc5c5a00f26" />


以上。
