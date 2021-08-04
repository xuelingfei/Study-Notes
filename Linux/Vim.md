## Vim


- [用户配置](#用户配置)  


#### 用户配置
配置 vim 的方法是在用户主目录下建立 .vimrc 文件，用户可以根据需求添加功能到自己的 .vimrc 中。  
常用配置：
> - set nocp(set nocompatible) -- 让 vim 工作在不兼容模式下
> - set ru(set ruler) -- 打开状态栏标尺
> - set hls -- 搜索时高亮显示被找到的文本
> - set is -- 搜索时在未完全输入完毕要检索的文本时就开始检索
> - syntax on -- 自动语法高亮（关键字上色）
> - set number -- 显示行号
> - set tabstop=4 -- 设定 tab 长度为 4
> - set shiftwidth=4 -- 设定 << 和 >> 命令移动时的宽度为 4
> - set softtabstop=4 -- 使得按退格键时可以一次删掉 4 个空格
> - set cursorline -- 突出显示当前行
> - set fileencoding -- 查看文件编码
```
set nocp
set ru
set hls
set is
syntax on
set number
set tabstop=4
set shiftwidth=4
set softtabstop=4
set cursorline
set fileencoding
```