PS2022 拖入图片后闪退，或对象选择偶尔闪退
Adobe官方解决方案：
找到并编辑 `%USERNAME%\AppData\Roaming\Adobe\Adobe Photoshop 2022\Adobe Photoshop 2022 Settings\PSUserConfig.txt` ，没有的话自己建一个，加入：
```
EnableDocumentGroup 0
UXPLearnAndSearch 0
```


